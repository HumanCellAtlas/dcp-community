### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# DCP DNS Design

Note: this RFC was approve under the pre-November, 2018 RFC process.

## Summary

This RFC defines the DNS structure of the HCA DCP.

## Author(s)

[Sam Pierson](mailto:spierson@chanzuckerberg.com)
[Tony Burdett](mailto:tburdett@ebi.ac.uk)
dvaughan

## Shepherd

[Mark Diekhans](mailto:markd@ucsc.edu)

## Motivation

Provide a single point of documentation of the DCP DNS structure.

## Detailed Design

The [humancellatlas.org](http://humancellatlas.org/) domain is controlled by Sanger IT.

[data.humancellatlas.org](http://data.humancellatlas.org/) is a zone registered in AWS Route 53 that the DCP team controls.

Sanger has added a record to their DNS configuration that delegates look-ups of *.data.humancellatlas.org* domains to the Route 53 nameservers. They have also added *schema.humancellatlas.org* to be added as a subdomain. The AWS Route 53 setting have been added.



subdomain               |    | name server
------------------------|----|------------
data.humancellatlas.org | NS | ns-202.awsdns-25.com
                        |    | ns-1127.awsdns-12.org
                        |    | ns-1554.awsdns-02.co.uk
                        |    | ns-812.awsdns-37.net
schema.humacellatlas.org | NS | ns-1071.awsdns-05.org
                         |    | ns-2004.awsdns-58.co.uk
                         |    | ns-339.awsdns-42.com
                         |    | ns-671.awsdns-19.net

DCP services for the production deployment live under the domain *.data.humancellatlas.org*. Services in pre-production deployments have the deployment name inserted before *.data.



### Production Deployment:

Service | hostname
--------|---------
Ingest Broker | ingest.data.humancellatlas.org
Ingest API | api.ingest.data.humancellatlas.org
Upload Service API | upload.data.humancellatlas.org
Data Store API | dss.data.humancellatlas.org
Secondary Analysis API | pipelines.data.humancellatals.org

### Staging Deployment (periodic releases, more stable):

Service | hostname
--------|---------
Ingest Broker | ingest.staging.data.humancellatlas.org
Ingest API | api.ingest.staging.data.humancellatlas.org
Upload Service API | upload.staging.data.humancellatlas.org
Data Store API | dss.staging.data.humancellatlas.org
Secondary Analysis API | pipelines.staging.data.humancellatals.org

### Development Deployment (ongoing development, unstable):

Service | hostname
--------|---------
Ingest Broker | ingest.dev.data.humancellatlas.org
Ingest API | api.ingest.dev.data.humancellatlas.org
Upload Service API | upload.dev.data.humancellatlas.org
Data Store API | dss.dev.data.humancellatlas.org
Secondary Analysis API | pipelines.dev.data.humancellatals.org



