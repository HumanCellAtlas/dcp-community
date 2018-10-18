# Upload Service

## Description
The Upload Service enables fast, cost-effective upload, staging, and validation of data submitted to the Human Cell Atlas (HCA) Data Coordination Platform (DCP) from across the world.

## Objectives
1. Enable submitters to upload files quickly and cost-effectively to temporary *upload areas* for subsequent submission to the Data Storage Service (DSS)
1. Provide tools a command line interface (CLI) and APIs to upload files to staging areas
1. Provide a framework for validating files loaded into staging areas with user-defined validation jobs

## In-scope
* Provide upload areas that enables data file upload and release to the DSS upon completion of:
    * Ingestion workflows
    * Secondary Analysis Service workflows via an Ingestion workflow
* Provide APIs for interacting with the upload service
    * Functionality for other DCP modules (primarily the DCP Ingestion Infrastructure)
        * Create and delete staging areas
        * Schedule and retrieve validation jobs for files
        * Store metadata content into staging area
    * Functionality for Submitters
        * Obtain credentials to write to upload areas
        * Examine the contents of upload areas
* Provide data integrity checks
    * Compute checksums for all staged files and make these checksums available to the DSS
    * Submitter must be able to cryptographically confirm that their data was transferred with integrity
* Run user-defined validation jobs on files
    * Provide environment to run containerized validation jobs on data files
    * Provide job scheduling, execution, and status notifications for jobs
* Provide tooling for submitters
    * Command line tools for uploading data into the system and system introspection
    * User authentication and authorization
* Provide libraries and tooling for automating and inspecting upload jobs
    * Event auditing for DCP operators enabling tracking job status
    * CI/CD automation for easing development and deployment
* Scale testing and optimization
    * Build applications to scale elastically with size of workload
    * Changes to the system to optimize cost and throughput
    * Data routing optimization and configuration that increases data transfer rates
    * Enable responsive workflow for data wranglers uploading datasets manually
* User experience design
    * Technical UX review with data wranglers and product iteration based on feedback
* Dependencies
    * User-defined jobs to validate files
    * Ingestion service (or other client) to track job state, request validations, and submit data

## Out-of-scope
* A UI that replicates the features in the DCP ingestion service
* Validation images (these are provided by users of the system)
* Upload Service libraries for programming languages unlikely to see use within the DCP or community

## Milestones and Deliverables
* End of H2 2018
    * REST HTTP API stable
    * Scale validated with metrics set by DCP PM
    * (stretch) develop plan to transition Upload Service to Ingest Module
* End of H1 2019
    * Transition to Ingest Module complete

## Roles

### Project Lead
[Parth Shah](mailto:pshah@chanzuckerberg.com)

### Product Owner
[Matt Weiden](mailto:mweiden@chanzuckerberg.com)

### Technical Lead
[Sam Pierson](mailto:spierson@chanzuckerberg.com)

## Communication

### Mailing List
Team email: upload-team@data.humancellatlas.org

### Slack Channels

[HumanCellAtlas/upload-service](https://humancellatlas.slack.com/messages/upload-service): Discussion of the Upload Service

## Github repositories

* https://github.com/HumanCellAtlas/upload-service - main upload service repository
