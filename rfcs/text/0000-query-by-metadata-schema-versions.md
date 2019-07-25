### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC
number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# Querying DSS by Metadata Schema Version(s)


## Summary

This RFC details a design to allow downstream components to modify their subscription to the DSS to be only notified
about bundles containing metadata within a specified schema version range. The selection of a solution is required to
unblock the metadata schema integration testing detailed here.

## Author(s)

[Arathi Mani](mailto:arathi.mani@chanzuckerberg.com)

## Shepherd

[Matt Weiden](mailto:mweiden@chanzuckerberg.com)

## Motivation

Today in the DCP when metadata schema makes change, we require that the entire system, including all independent
downstream components (i.e. Expression Matrix service, Secondary Analysis, Query service, Data Browser, etc.) are
compatible with the changes before we allow the metadata team to push their changes through. This causes extreme
slowness in metadata schema updates which is non-ideal due to the fact that schema changes are frequent and prevents new
data from being added to the DCP as the new data is likely to adhere to the changed metadata schema.

In order to enable more frequent metadata schema change-related pushes to production, a [metadata schema integration 
test design](https://docs.google.com/document/d/18C_Wbrr4Huc8yxQXSdNbzeGk0glBjkfq0MGnozwlIB8/edit) has been proposed. 
One part of design is to require downstream components to modify their production subscriptions to the Data Store 
Service (DSS) such that they only receive notifications about bundles with which they are compatible. This will allow
wranglers to submit new data to the DSS without having to wait for compatibility of the whole system and also allows
downstream components to receive data independent of whether another component is ready as well. Due to the fact that
there is currently a metadata schema freeze disallowing any changes, major, minor, or patch, to be pushed to production,
this document simply designs the minimum system required to unblock wranglers from submitting data and protecting
downstream components (and therefore the entire DCP) from breaking due to incompatibility.

Note: the subscription modifications will only exist in the production environment. Once the metadata schema integration
tests are setup, subscriptions will be modified to remove any restrictions on metadata schema versions in order to
eagerly surface incompatibilities.


### User Stories

*Share the [User Stories](https://www.mountaingoatsoftware.com/agile/user-stories) motivating this RFC.*
* As a data wrangle, I would like to be able to push new data adhering to the latest metadata schema without having to
wait for every single component to be compatible with the new schema versions.
* As a DCP developer working on a downstream component (e.g. Matrix Service, Secondary Analysis, etc.) I would like to
receive notification about new data adhering to new metadata schema versions as soon as I am ready without having to
wait for other DCP components to also be compatible. 

## Detailed Design

There are two questions fueling the design of this system:

1) How will the schema version be noted in the metadata schema?
2) Will ElasticSearch, JMESPath, and Query Service all be able to issue range queries against the format of the schema
version?

Today, the version of the metadata schema is embedded in the `described_by` field which contains the schema version
embedded in the URL; an example value is “https://schema.humancellatlas.org/type/biomaterial/15.3.0/donor_organism” and
the schema version is 15.3.0. Unfortunately embedding the version within the URL does not facilitate searching for a
range of data between schema versions. 

### New schema version fields in metadata schema
We propose adding two new required fields to the provenance schema which is embedded into each Type schema:

```
"schema_major_version": {
      "description": "The major version number of the schema.",
      "type": "integer",
      "minimum": 1,
      "example": "4"
},
"schema_minor_version": {
      "description": "The minor version number of the schema.",
      "type": "integer",
      "example": "6"
}
```

We have noticeably left off adding the patch version; this is due to the fact that patch version increments only affect
documentation about the metadata and therefore can have no effect on any code that uses the metadata schema. Since it
does not affect forward or backwards compatibility we have chosen not to represent it explicitly in the schema which
means that subscribing to data based on the patch version is not supported.

We have decided not to represent the full version as `x.y.z` in the schema_version because formatting in this way is not
compatible with the way that ElasticSearch performs range queries. Range queries in ElasticSearch can only be done on
fields that are either an integer or a floating point number (or a datetime).

### ElasticSearch Query for Metadata Schema Version
```
# Query for files where the metadata schema version of the project_json metadata schema # is greater than or equal to
# 1.0 but less than 3.2
query = {
  "query": {
    "bool": {
      "must": {
        "term": {
          "files.project_json.provenance.document_id": project_uuid
        }
      },
      "should": [
        {
          "bool": {
            "must": {
              "range": {
                "files.project_json.provenance.schema_major_version": {
                  "gte": 1,
                  "lte": 2
                }
              }
            }
          }
        },
        {
          "bool": {
            "must": [
              {
                "term": {
                  "files.project_json.provenance.schema_major_version": 3
                }
              },
              {
                "range": {
                  "files.project_json.provenance.schema_minor_version": {
                    "gte": 0,
                    "lte": 2
                  }
                }
              },
            ]
          }
        }
      ],
      "minimum_should_match": 1
    }
  }
}
```

Noticeably, ES queries can get long-winded very quickly especially given that there are up to 25 schemas for which
versions can be specified. However, ElasticSearch queries are actively being deprecated, tracked by ticket 
[here](https://app.zenhub.com/workspaces/dcp-5ac7bcf9465cb172b77760d9/issues/humancellatlas/data-store/906), and will 
require components that currently use ElasticSearch queries for their subscriptions to switch to either JMESPath or
Query Service.  

### JMESPath Query for Metadata Schema Version

```
# Query for files where the metadata schema version of the library_preparation_protocol is greater than or equal to 1.0
# but less than 3.2
jmespath_query = "(event_type === `CREATE`) && ((file.library_preparation_protocol[].schema_major_version >= 1 && \
    file.library_preparation_protocol[].schema_major_version <= 2) ||
    (file.library_preparation_protocol[].schema_majorversion == 3 && \
    file.library_preparation_protocol[].schema_minor_version <= 2))"

```

### Query-Service Query for Metadata Schema Version
Since Query Service is currently not used as part of any downstream components’ subscriptions to the DSS, [a ticket has
been filed](https://github.com/HumanCellAtlas/query-service/issues/221) to add functionality such that querying for data
based on a range of metadata schema versions will be enabled in the future *by the time ElasticSearch is deprecated*.

### Implementation
The following are the implementation requirements in order to fulfill the design above. The work is split into two
phases to enable first quickly unblocking wranglers from submitting data due to the current metadata schema freeze and
then second the other portions of work that will be required to ensure that this design is airtight for the long term.

**Phase 1 (in implementation order)**

1) Add the above two required fields to metadata schemas. DO NOT make them a required field yet as all data currently in
the DCP do not have these fields populated and therefore will be inaccessible by the downstream components if they were
required.
2) Make changes in Ingest such that when parsing the `describedBy` field, `schema_major_verion` and 
`schema_minor_version` are populated as well. Add validation to make sure the values in the new fields and `describedBy`
do not differ.
3) Make changes in Secondary Analysis code such that when the secondary bundles are being created and the `describedBy`
field is populated, the `schema_major_verion` and `schema_minor_version` fields are also populated with the same schema
version information. The same validation as step 2 should trigger to make sure there is no diff in the values.
4) **Optionally** update the subscriptions of Azul (/Data Browser), Matrix Service, Query Service, and Secondary 
Analysis to reflect the ranges of schemas that they are compatible with. If they are agnostic to the version of a
particular schema (for example, if they do not use the metadata schema properties in any way so its formatting doesn’t
matter), then there is no need to specify the versions. Components may choose to opt out of specifying any versions if
they have built-in logic to handle any errors related to metadata schema changes.

Note about updating subscriptions: Components that use [metadata-api](https://github.com/humancellAtlas/metadata-api)
must restrict the version range on every schema listed 
[here](https://github.com/HumanCellAtlas/metadata-api/blob/master/src/humancellatlas/data/metadata/api.py#L792).

At this point, new data may be submitted to the DSS by wranglers adhering to a new schema version.

**Phase 2**

1) Back-populate the schema versions in all existing data via either a simple script or migration tool. Re-ingest if
needed (but hoping that this will not be necessary if [simple updates are 
implemented](https://app.zenhub.com/workspaces/dcp-5ac7bcf9465cb172b77760d9/issues/humancellatlas/dcp/222)). 
2) Set the two fields as now being required.

At this point, the MVP of this design is complete.

**Post MVP**

At some point ElasticSearch will be deprecated and replaced by Query Service. Before this happens, Query Service should
be able to handle querying by ranges of schema versions.

***Future Work***
Transitioning towards a versioning system for metadata schemas where one version represents the state of the entire set
of metadata schemas might be a viable solution to look into by the end of the year (2019). This would prevent the
necessity of subscriptions specifying potentially versions for multiple schemas which make the queries quite cumbersome.

Another "nice-to-have" would be an automated generation of subscriptions to also lessen the complexity in drafting the
appropriate subscriptions to adhere to potentially many schema versions. 


### Acceptance Criteria

* The current metadata schema version freeze is lifted and the metadata schema changes listed
[here](https://docs.google.com/spreadsheets/d/1B52QjioJ7CzOLXFIcB-Q8mfjiFSH0jzbfdNs_nce3P4/edit#gid=0) are pushed to
production.
* Data wranglers are able to contribute data with metadata that adheres to the new schema versions.

### Prior Art

* The initial design was written in [a Google 
Doc](https://docs.google.com/document/d/1BInKossmrlpna5MV_s4MSR0Bll1wnD-3OVw-9nCLdWg/edit?ts=5d389d03#) which is mostly
a replica of the content in this RFC.
* This work is part of a larger piece of work around introducing [metadata schema integration
testing](https://docs.google.com/document/d/18C_Wbrr4Huc8yxQXSdNbzeGk0glBjkfq0MGnozwlIB8/edit#). The end goal beyond the
scope of this RFC is to preemptively be able to detect breakages in the system due to metadata schema changes and
automatically push updates when the overall DCP system is compatible. This also involves work around establishing a
migration SOP and building out the [Metadata API](https://github.com/humancellAtlas/metadata-api).
* [Slide deck](https://tinyurl.com/y2fljppe) presented to DCP PMs on Engineering Plan to unfreeze metadata schema 
versions.
