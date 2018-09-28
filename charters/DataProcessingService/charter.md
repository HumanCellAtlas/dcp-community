# [Data Processing Service](mailto:pipelines-team@data.humancellatlas.org)

## Description
The data processing service is a service that executes automated transformation to scientific data through cloud-based execution of scientific pipelines. It can be used in any instantiation of the Data Coordination Platform (DCP). 

## Objectives
The data processing service aims to develop the infrastructure to run transformative scientific analysis pipelines reliably and at-scale for an instantiation of the DCP. The team working on the service will also operate the infrastructure for the Human Cell Atlas. 

## In-scope
* Creating and maintaining the Data Processing Service
* Operating the instantiation of the HCA DCP Data Processing Service  
* Initiation and management of the execution of all data processing pipelines that run in the HCA DCP  
* Documentation on the Data Processing Service and guides on using the service 
* Establishing and operating automated practices around engineering testing of pipelines in the HCA DCP 
* Creating and outputting metadata on the transformations done to the data in accordance with the Metadata Schema

## Out-of-scope
* Standing up or operating other instantiations of the Data Processing Service

## Milestones and Deliverables
__2018:__ Create operational and secure infrastructure, add ability to throttle and queue.  
__2019:__ Add ability to reanalyze data, notification filtering to save costs, improve testing, logging, and deployment.  
__2020:__ Generalized and reusable service infrastructure.  

## Roles
### Project Lead
[Timothy Tickle](mailto:ttickle@broadinstitute.org)

### Product Owner
[Kylee Degatano](mailto:kdegatano@broadinstitute.org)

### Technical Lead
[David Shiga](mailto:dshiga@broadinstitute.org)

## Communication
[Team email](mailto:pipelines-team@data.humancellatlas.org): pipelines-team@data.humancellatlas.org
__HumanCellAtlas#mintteam:__ team discussions, a place to directly communicate to the team, who works on the service and the pipelines, as needed  
__HumanCellAtlas#mintteam-ci:__ mint team continuous integration status

## Github repositories
[Cromwell Tools](https://github.com/broadinstitute/cromwell-tools): Tools (Python API) for working with the Cromwell workflow engine 
[Falcon](https://github.com/HumanCellAtlas/falcon): Throttles workflows and starts queued workflows 
[Lira](https://github.com/HumanCellAtlas/lira): Listens to storage service notifications and launches workflows
[Pipeline Tools](https://github.com/HumanCellAtlas/pipeline-tools): Data Coordination Platform adapter pipelines and associated tools  
[Secondary Analysis](https://github.com/HumanCellAtlas/secondary-analysis): Integration test scripts and other automation for the Data Processing Service. Contains all tickets for the prioritized work for the implementation team 
