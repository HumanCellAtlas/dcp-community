
# Project Name
HCA Data Processing Service

## Description

The data processing service is a centralized space to perform all transformative functionality to scientific data ingested into the Human Cell Atlas Data Coordination Platform (HCA DCP).

## Definitions

__Community pipeline:__ A pipeline that is not officially used as a standard in the HCA project but runs in the DCP and is operated by the Data Processing Service to enable community members to work with a diversity of submitted data until they have a standardized pipeline. These pipelines are not as vetted as standardized pipelines.  
__HCA Analysis Working Group (AWG):__ A group of domain specialists that advise on and monitor scientific choices made in the Human Cell Atlas Project.  
__HCA DCP GG:__ A working group of the Human Cell Atlas project that advises the DCP, assuring alignment of DCP development efforts with the scientific project it supports.  
__On-boarding:__ The process of taking an existing pipeline and engineering it to work in the HCA DCP.  
__Scoping:__ The process of investigating the available resources and knowledge in a community around a specific assay to better understand how much work it would take to make a pipeline to process data from that assay.  
__Standardized Pipeline:__ An official pipeline of the HCA project.  

## Objectives

The data processing service aims to develop scientific analysis pipelines and the infrastructure to run them reliably and at-scale for all analyses on all scientific data types in the Human Cell Atlas Data Coordination Platform. The team working on the service will also operate the infrastructure. Pipelines for additional data types, analyses, and improvements to these pipelines will be added iteratively and in collaboration with the HCA Analysis Working Group (AWG) and the community.

## In-scope

* Scoping or review of scoping for new pipelines.  
* Creation of all standardized pipelines for scientific data.  
* Review, on-boarding, and potential creation of community pipelines.  
* Review of community tools for use in data processing pipelines.  
* Updating pipelines ran in the HCA DCP for optimization of cost, time, or scientific benchmarks.  
* Establishing standard benchmarking used to evaluate data processing pipelines.  
* Creation, maintenance, and operation of the HCA DCP pipeline execution environment.  
* Investigating and identifying data for pipeline benchmarking.  
* Initiation and management of the execution of all data processing pipelines (community or standardized) that run in the HCA DCP.  
* Evaluation and adoption of file formats (and associated standards) for outputs of pipelines.  
* Establishing and operating automated practices around scientific and engineering testing of pipelines.  
* Outreach to users on the usability of data in downstream applications.  
* Outreach to developers on pipeline development in the HCA DCP.  
* Suggestion but not selection of new standardized pipelines for the HCA DCP.  

### Scientific "guardrails" [Optional]

* Initial versions of standardized pipelines are reviewed by the AWG.  
* Changes to standardized pipelines that affect the scientific output are reviewed by the AWG, engineering updates that do not affect the scientific output are not reviewed by the AWG.  
* Selection of standardized pipelines to work on are approved by the HCA DCP Governance Group (this does not apply to community pipelines, community pipelines are on-boarded as a service to the scientific community). Prioritization of the work to develop selected pipelines is performed by the data processing service team.  

## Out-of-scope

* Educational training or user assistance on how to perform data analysis with outputs of pipelines.  
* Recommendations or demonstrations of downstream tool use beyond initial data loading.  
* Novel methodological development is not performed by the data processing team unless it is essential to the pipeline, it does not existent in community tools, and it represents minimal effort to implement.  
* Software engineering 3rd party tools (although may happen in high value community tools, is not a requirement of the service and left to the discretion of the team).  

## Milestones and Deliverables
__2018:__ Infrastructure operational and secure, two data processing pipelines in production (Smart-seq2 and 10xv2 human data).  
__2019:__ Addition of at least three more data processing pipelines, including 10xv2 snRNA-seq and mouse pipelines for all current analysis pipelines. Aim to have at least one _in situ_ transcriptomics pipeline in this year. Ability to reanalyze data added.  
__2020:__ Generalized and reusable service infrastructure. Continued addition of analysis pipelines.  

## Roles
### Project Lead
[Timothy Tickle](mailto:ttickle@broadinstitute.org)

### Product Owner
[Kylee Degatano](mailto:kdegatano@broadinstitute.org)

### Technical Lead
[David Shiga](mailto:dshiga@broadinstitute.org)

## Communication
__HumanCellAtlas#mintteam:__ team discussions, a place to directly communicate to the team, who works on the service and the pipelines, as needed.  
__HumanCellAtlas#mintteam-ci:__ mint team continuous integration status.  
__HumanCellAtlas#secondary-analysis:__ community-focused space for conversations around data processing pipelines that run in the DCP.  

## Github repositories
[Cromwell Tools](https://github.com/broadinstitute/cromwell-tools): Tools (Python API) for working with the Cromwell workflow engine.  
[Falcon](https://github.com/HumanCellAtlas/falcon): Throttles workflows and starts queued workflows.  
[Lira](https://github.com/HumanCellAtlas/lira): Listens to storage service notifications and launches workflows.  
[Pipeline Tools](https://github.com/HumanCellAtlas/pipeline-tools): Data Coordination Platform adapter pipelines and associated tools.  
[scTools](https://github.com/HumanCellAtlas/sctools): Tools for single cell data processing.  
[Secondary Analysis](https://github.com/HumanCellAtlas/secondary-analysis): Integration test scripts and other automation for the Data Processing Service. Contains all tickets for the prioritized work for the implementation team.  
[Skylab Analysis](https://github.com/HumanCellAtlas/skylab-analysis): Analysis and benchmarking reports for Skylab pipelines.  
[Skylab](https://github.com/HumanCellAtlas/skylab): HCA data processing pipelines.  
