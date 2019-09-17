# [Pipeline Execution Service](mailto:pipelines-team@data.humancellatlas.org)

## Description

The Pipeline Execution Service executes data processing pipelines containing analysis tasks performed on scientific data in the Data Coordination Platform (DCP). The outputs of these pipelines are used for scientific analysis and interpretation.  

## Objectives

The Pipeline Execution Service runs pipelines reliably and at-scale for the DCP. The team implementing the service also operates the pipeline infrastructure for the Human Cell Atlas project. 

## Definitions

__pipeline:__ A collection of one or more functional tasks that operate on input data and, from that, transform the input data, or derive features often used to interpret the input data. In a high throughput setting, these tasks are often automated to be performed in a batch setting.

## In-scope

### Operations

* Operating the instance of the HCA DCP Pipeline Execution Service  
* Initiation and management of the execution of all data processing pipelines that run on scientific data in the HCA DCP  
* Establishing and operating automated practices around engineering testing of pipelines executed in the HCA DCP Pipeline Execution Service 

### Documentation

* Creating and outputting metadata on the operations applied to the data in accordance with the HCA DCP Metadata Schema  
* Documentation on the Pipeline Execution Service and guides on using the service  
* Creating documentation and scripting around the deployment of the Pipeline Execution Service

## Milestones and Deliverables

__2018:__ Create operational and secure infrastructure, add ability to throttle and queue the executions of data processing pipelines/workflows  
__2019:__ Add ability to reanalyze data, notification filtering to save costs, improve testing, logging, and deployment  
__2020:__ Generalized and reusable service infrastructure  

## Roles

### Project Lead

[Timothy Tickle](mailto:ttickle@broadinstitute.org)  

### Product Owner

[Janki Kaneria](mailto:jkaneria@broadinstitute.org)  

### Technical Lead

[Saman Ehsan](mailto:sehsan@broadinstitute.org)  
[Rex Wang](mailto:chengche@broadinstitute.org)  

## Communication

### Slack Channels

[HumanCellAtlas/data-pipelines-team](https://humancellatlas.slack.com/messages/data-pipelines-team): team discussions, a place to directly communicate to the team that works on the execution service and scientific pipelines  
[HumanCellAtlas/data-pipelines-ci](https://humancellatlas.slack.com/messages/data-pipelines-ci): Pipeline Execution Service continuous integration status  

### Mailing Lists

Team email: pipelines-team@data.humancellatlas.org 

## Github repositories

[Cromwell Tools](https://github.com/broadinstitute/cromwell-tools): A collection of Python clients and accessory scripts for interacting with the [Cromwell workflow execution engine](https://github.com/broadinstitute/cromwell) - a scientific workflow engine designed for simplicity and scalability  
[Falcon](https://github.com/HumanCellAtlas/falcon): Queueing system that (after launching) throttles and inititates workflows  
[Lira](https://github.com/HumanCellAtlas/lira): Listens to storage service notifications and launches workflows  
[Pipeline Tools](https://github.com/HumanCellAtlas/pipeline-tools): Contains Data Coordination Platform adapter pipelines and associated tools  
[Secondary Analysis](https://github.com/HumanCellAtlas/secondary-analysis): Integration test scripts and other automation for the Pipeline Execution Service (contains all tickets for the prioritized work for the implementation team)  
[Secondary Analysis Deploy](https://github.com/HumanCellAtlas/secondary-analysis-deploy): Contains the deployment configuration and scripts for the Pipeline Execution Service 
