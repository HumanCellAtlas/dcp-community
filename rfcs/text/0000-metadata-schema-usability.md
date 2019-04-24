### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# Metadata schema usability

## Summary

*Describe the vision of the RFC in 2-3 sentences. Consider this section as your "elevator pitch" or "release note" to the community.*

An approach to generating a user-friendly version of the metadata JSON schema from the modularized source is proposed.  These *demodularized* schemas will be built from the current source and be the standard user view of the data model.   This addresses issues with the schema structure being difficult to understand and with field names and descriptions defined in modules not reflecting the user-visible instance data type.

## Author(s)

*Recommended format for Authors:*

 [Mark Diekhans](mailto:markd@ucsc.edu).
 [Simon Jupp](mailto:jupp@ebi.ac.uk),
 [Mallory Freeberg](mailto:mfreeberg@ebi.ac.uk),
 [Matt Green](mailto:hewgreen@ebi.ac.uk),
 [Dani Welter](mailto:dwelter@ebi.ac.uk),
 [Tony Burdett](mailto:tburdett@ebi.ac.uk)

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*
 
 `[Name](mailto:username@example.com)`

## Motivation

*Describe the user or technical need in detail [with alignment to the DCP roadmap priorities where possible]. Link prior community discussions to demonstrate support for this RFC.*

Consumers of the HCA metadata JSON Schema have found that the module structure makes the schema difficult to understand, even for experienced programmers. Currently, to support 26 different instance types, there are 78 JSON Schema files.  While not an overly large number of modules, the composition model of JSON schema does not match any common paradigm, making it difficult to explain.  With no tools available for visualizing or understanding JSON Schema, which makes the HCA data model opaque.

Another result of the module structure is that field names and descriptions defined in modules don't match the concrete class.  This makes schema-driven users interfaces confusing. 

JSON schema does not support inheritence, so any modules that would ideally be inhereted by all schemas and contain intrinsict properties, are currently referenced using a nested property. This mostly applies to all `*_core` modules that should be inhereted by schemas rather than referenced using a nested property. 

There are no rules governing module usage so it is not clear to metadata developers when modules should be created. For this reason modules are used in different ways throughout the metadata schemas. 

However, the module structure adds value in making the schema easier to create, maintain, and keep consistent by avoiding redundant specifications of fields.

### User Stories

*Share the [User Stories](https://www.mountaingoatsoftware.com/agile/user-stories) motivating this RFC.*

* As a consumers of the HCA metadata JSON Schema I want to be able to be able intuitively interpret schema based on common paradigms so that I can easily maintain and extend them.
* As a metadata schema developer I want to be able to interpret schemas locally without reading other documents so that I can easily maintain and extend them.
* As a metadata schema developer I want schemas to have no duplicated fields so that I don't have to edit a field in multiple places when making a change.


## Detailed Design

*Explain the design in sufficient detail such that the implementation and (if appropriate) the interaction with existing DCP software are both reasonably clear.*

We are prpopsoing to fully expand schemas at publishing times so consumers are only exposed to 'type' schemas. Core fields would be copied into these type schemas based on the `schema_type` and module objects would be expanded in-place. Consumers would see all the infomration in one place without having to de-references sections of the schema. 

### Current JSON schema example


The `donor.json` schema is a type schema that has a `$ref` to a `biomaterial_core` module. 
```
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "properties": {
        "biomaterial_core" : {
            "type": "object",
            "$ref": "core/biomaterial/biomaterial_core.json",
            "user_friendly": "Donor"
        }
    }
}
```

Here's a snippet from the `biomaterial_core.json` module
```
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "properties": {
        "biomaterial_id":{
            "description": "A unique ID for the ${id}.",
            "type": "string",
            "user_friendly": "${id} ID"
        }
      }
}

```

### Proposed JSON schema after running the publishing script 

The `biomaterial_core.json` module is never published to users and users just see a `donor.json` schema. The `user_friendly` 
name and `description` has been contextualised as part of the publishing script. 

```
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "properties": {
      "biomaterial_core" : {
        "properties": {
          "biomaterial_id":{
              "description": "A unique ID for the Donor.",
              "type": "string",
              "user_friendly": "Donor ID"
          }
          }
      }
    }
}
```

This implementation makes schemas easier to interpret without needing to look in other referenced core and module schema documents. The source schema representation would be reserved for editors (like source code), and would still allow for modularisation to avoid duplication. Tooling would be required that compiled to the schemas into a published format that is designed to be more user friendly. The comliation process would allow for additonal manipulation of the schema to produce context specific user friendly names and descriptions for fields. For example, presenting the `biomaterial_id` field in the donor schema as "Donor id" in user facing tools such as spreadsheets and the data browser.  

In order to align and manage module usage, the current modules should be revised in order to meet the following criteria:

1. By default nothing is a module.
1. A group of fields can become a module if they are used in more than three places (e.g. the timecourse module). This rule also applies to fields that are semantically identical but differ in name due to the context in which they are used (e.g. ${VARIABLE}_id -> donor_organism_id).
1. Related fields that require multiple values should be nested arrays in the schema, but they should be expanded in the type document rather than made into a separate module.


### Acceptance Criteria [optional]

*Acceptance criteria are the conditions that a RFC must satisfy to be accepted by users or other stakeholders.* 

### Unresolved Questions

- *Should ontology modules remain as is or should they also be expanded in-place?*



### Drawbacks and Limitations [optional]

*Why should this RFC **not** be implemented?*

**Schemas are much larger**
However, this is not likely to make schemas harder to interpret.

**Duplication of fields**
We would require tooling for updating fields that exist in multiple places.

### Prior Art [optional]

*Share references to prior art to deepen community understanding of the RFC, such as learnings, adaptations from earlier designs, or community standards.*

### Alternatives [optional]

*Highlight other possible approaches to delivering the value proposed in this RFC. 
What other designs were explored? What were their advantages? What was the rationale for rejecting alternatives?*


