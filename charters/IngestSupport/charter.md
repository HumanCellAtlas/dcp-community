# Ingest Support

## Description

The Ingest Support team is the liaison between the Data Coordination Platform (DCP) and labs contributing data to the Human Cell Atlas (HCA). This team closely collaborates with each data contributor to:

*	Working with data contributors who are generating new data types to develop a deep understanding of their data
* Assisting data contributors to describe their experiments in a manner that supports finability, reusability and reproducability with in the DCP ecosystem
* Helping data contributors to submit their data to the DCP, resolving any emerging issues during the process

## Definitions

*Data contributor* - any user who uploads data and metadata into the DCP

*Data wrangler* - a member of the ingest support team who assists to data contributors wanting to upload data into the DCP

*Submission* - the grouping of data and metadata that makes sense to a data contributor. For example, a data contributor might typically upload all the data at once for a publication they have in preparation. This grouping does not necessarily correspond to how data is stored or displayed in the DCP and how it should be displayed to a data consumer

*Data* - raw data files from an experiment such as fastq files for sequencing experiments or tiff stacks for imaging experiments

*Metadata* - descriptive information about what projects, samples and experiments are and how they were collected or conducted

*New data types* - experimental data types which either can't already be ingested by the DCP or have only recently (~6m - 1yr) started to be ingested by the system

*Established data types* - experimental data types which have a history of submission and downstream use in the DCP and there are already many datasets of this type represented in the platform

## Objectives

- Assisting data contributors uploading data and metadata to the DCP
- Helping data contributors fix errors with their data and metadata before it can be submitted to the DCP
- Ensuring a data contributor's experiment can be accurately represented in the DCP
- Working with the data contributor to propose extensions to the HCA metadata schema if additional metadata fields are required
- Working with the metadata team to review proposed metadata extensions to ensure they meet the needs of data contributors
- Working with the UX team to collect and provide feedback from data contributors to the ingest infrastructure team about how the infrastructure can be improved
- Working with the UX team to ensure the ingest process provides a positive user experience to the data contributors

## In-scope

### For new data types

- Working with data contributors to define a spreadsheet template to collect their metadata
- Collecting metadata from data contributors in HCA ingest spreadsheet form
- Collecting data files from data contributors
- Reviewing collected data files and metadata
- Submitting data files and metadata to the DCP on behalf of a data contributor
- Working with data contributors to fix errors in the data and metadata

### For established data types

- Assisting data contributors uploading data files to the platform
- Assisting data contributors using the Ingest User interface to generate a metadata template spreadsheet
- Assisting data contributors uploading metadata to the platform
- Assisting data contributors who wish to use the Ingest API to submit data
- Working with data contributors to fix errors in their data and metadata

### User Experience

- Collecting feedback about the submission process from data contributors
- Working with the UX team to use feedback to define new requirements for the system to be past to the PM team

### Documentation and Training

- Writing training material about how to submit data to the DCP
- Responding to data contribution requests on Zendesk

### Scientific "guardrails" 

The ingest support team will work with the HCA DCP PM team and the HCA scientific leadership to understand the data type priorities for the HCA and help communicate those to potential data contributors

## Out-of-scope

- Implementing code for the ingest infrastructure system
- Actively submitting data on behalf of data contributors with established data types
- Consistently reformatting data for a data contributor with an established data type when it does not meet HCA requirements
- Updating the HCA metadata schemas 
- Answering HCA helpdesk questions that come to data-help@humancellatlas.org about non-data contribution topics

## Milestones and Deliverables


## Roles

### Project Lead

[Clay Fischer](mailto:clmfisch@ucsc.edu)

### Product Owner

[Norman Morrison](mailto:norman@ebi.ac.uk)

### Process Lead

[Mallory Freeberg](mailto:mfreeberg@ebi.ac.uk)

## Communication

[HumanCellAtlas/#ingestion-service](https://humancellatlas.slack.com/messages/ingestion-service) for discussions of the Ingestion service

[HumanCellAtlas/data-wrangling](https://humancellatlas.slack.com/messages/data-wrangling) for discussion about the data wrangling process

## Github repositories

[https://github.com/HumanCellAtlas/ingest-central](https://github.com/HumanCellAtlas/ingest-central)
ingest-central is the hub repository for the Ingestion Service - use this to report issues or request new features in the ingest service
