# Upload Service

## Description
The Human Cell Atlas (HCA) Data Coordination Platform (DCP) Upload Service provides a file staging and validation facility. Upload Areas are created and deleted using a [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) Application Programming Interface (API), which is secured so only the DCP Ingestion Service may use it. It stages files into cloud storage and computes checksums for the files. The validation service can run arbitrary validation jobs against files.

## Objectives
1. Provide temporary storage for staging of files by submitters during ingestion into DCP Ingestion Service and subsequent storage in the Data Storage Service (DSS)
1. Provide tools a command line interface (CLI) and API to upload files to staging areas
1. Provide a facility to validate files by running an arbitrary workload against the file to be used by Ingest for validation

## In-scope
* Staging Areas - provide file staging areas to be used to marshal data and metadata files
    * Efficient file transfer to the DCP-DSS, the data’s final location
    * Efficient file transfer from submitters in remote geographic locations
    * Efficient file transfer from the DCP Secondary Analysis Service (which deposits results in GCS)
* HTTP REST API
    * Functionality for other DCP modules, namely Ingest
        * Request creation and deletion of staging areas
        * Schedule and retrieve validation jobs for files
        * Store metadata content into staging area
    * Functionality for Submitters
        * Obtain credentials to write to upload areas
        * Examine the contents of upload areas
* Checksumming
    * Compute `SHA-1`, `SHA-256`, `CRC-32C` and `S3 ETAG` checksums for all staged files and make these checksums available to the DSS
    * Submitter must be able to cryptographically confirm that their data was transferred with integrity
* CLI interface to submitters
    * Command line tools for uploading data into the system and system introspection
    * User authentication and authorization
    * Parallel upload of multiple files
* Validation
    * Provide environment to run containerized validation jobs on data files accepted by the DCP
    * Provide a base container image upon which validator writers will build their validators
* Scale testing and optimization
    * Build applications to scale elastically with size of workload
    * Distributed tracing, profiling, and code optimization
    * Changes to the system to optimize cost
    * Data routing optimization and configuration that increases data transfer rates
    * Enable “timely” workflow for data wranglers uploading datasets manually
* Libraries and Tooling
    * An auditing system for tracking job status through the system
    * Libraries in languages commonly used in the DCP for interfacing with the HTTP REST API
    * CI/CD automation for easing development and deployment
* User experience design
    * New features should be driven by user stories
    * Technical UX review with data wranglers and product iteration based on feedback
* Dependencies
    * Validation docker images from API users scheduling validation jobs

## Out-of-scope
* A UI that replicates the features in the DCP ingestion service
* Validation images (these are provided by users of the system)
* Upload Service libraries for programming languages unlikely to see use within the DCP or community

## Milestones
* EOY 2018 - Analysis can upload from GCS; scale validated with metrics set by DCP PM; (stretch) develop plan to transition Upload Service to EBI
* EH1 2019 - Transition to EBI complete; upload from browser

## Roles

Recommended format for Roles: `[Name](mailto:username@example.com)`

### Project Lead
[Parth Shah](mailto:pshah@chanzuckerberg.com)

### Product Owner
[Matt Weiden](mailto:mweiden@chanzuckerberg.com)

### Technical Lead
[Sam Pierson](mailto:spierson@chanzuckerberg.com)

## Communication

### Slack Channels

* HumanCellAtlas/upload-service: Discussion of the Upload Service

## Github repositories

* https://github.com/HumanCellAtlas/upload-service
