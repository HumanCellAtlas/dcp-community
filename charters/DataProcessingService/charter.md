# [Data Processing Service](mailto:pipelines-team@data.humancellatlas.org)

## Description

The Data Processing Service is a service that executes data processing pipelines containing scientific analysis on scientific data in the Data Coordination Platform (DCP).  

## Objectives

The Data Processing Service develops the infrastructure to run pipelines reliably and at-scale for the DCP. The team implementing the service also operates the pipeline infrastructure for the Human Cell Atlas project. 

## Definitions

__pipeline:__ A collection of one or more functional tasks that operate on input data and, from that, make an output that is transformed input data, or features generated to interpret the input data.

## In-scope

* Creating and maintaining the Data Processing Service  

### Operations

* Operating the instance of the HCA DCP Data Processing Service
* Initiation and management of the execution of all data processing pipelines that run on scientific data in the HCA DCP  
* Establishing and operating automated practices around engineering testing of pipelines executed in the HCA DCP Date Processing Service   

### Documentation

* Creating and outputting metadata on the operations done to the data in accordance with the HCA DCP Metadata Schema  
* Documentation on the Data Processing Service and guides on using the service  
* Creating documentation and scripting around the deployment of the Data Processing Service

## Out-of-scope

* Standing up or operating other instances of the Data Processing Service for other projects  

## Milestones and Deliverables

__2018:__ Create operational and secure infrastructure, add ability to throttle and queue  
__2019:__ Add ability to reanalyze data, notification filtering to save costs, improve testing, logging, and deployment  
__2020:__ Generalized and reusable service infrastructure  

## Roles

### Project Lead

[Timothy Tickle](mailto:ttickle@broadinstitute.org)  

### Product Owner

[Kylee Degatano](mailto:kdegatano@broadinstitute.org)  

### Technical Lead

[Saman Ehsan](mailto:sehsan@broadinstitute.org)  
[Rex Wang](mailto:chengche@broadinstitute.org)  

## Communication

[Team email](mailto:pipelines-team@data.humancellatlas.org): pipelines-team@data.humancellatlas.org 
 
__HumanCellAtlas#mintteam:__ team discussions, a place to directly communicate to the team, who works on the service and the pipelines, as needed  
__HumanCellAtlas#mintteam-ci:__ mint team continuous integration status  

## Github repositories

[Cromwell Tools](https://github.com/broadinstitute/cromwell-tools): Tools (Python API) for working with the [Cromwell workflow engine](https://github.com/broadinstitute/cromwell) - a scientific workflow engine designed for simplicity and scalability_  
[Falcon](https://github.com/HumanCellAtlas/falcon): Queueing system that (after launching) throttles and inititates workflows  
[Lira](https://github.com/HumanCellAtlas/lira): Listens to storage service notifications and launches workflows  
[Pipeline Tools](https://github.com/HumanCellAtlas/pipeline-tools): Contains data Coordination Platform adapter pipelines and associated tools  
[Secondary Analysis](https://github.com/HumanCellAtlas/secondary-analysis): Integration test scripts and other automation for the Data Processing Service (contains all tickets for the prioritized work for the implementation team)  
