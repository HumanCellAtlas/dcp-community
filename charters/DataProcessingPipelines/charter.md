# [HCA Data Processing Pipelines](mailto:pipelines-team@data.humancellatlas.org)

## Description

The HCA Data Processing Pipelines are cloud-based and portable analysis pipelines that perform transformative analysis on scientific data in the HCA Data Coordination Platform.

## Objectives

The HCA Data Processing Pipelines team develops all pipelines required by the HCA project for all transformative analyses on scientific data types in the HCA Data Coordination Platform (DCP). Pipelines created to respond to the addition of data types and assays and improvements to pipelines will be added iteratively and in collaboration with the HCA Analysis Working Group (AWG) and the community. 

## Definitions

__Community pipeline:__ A pipeline that is not officially used as a standard in the HCA project, but runs in the DCP and is operated by the Data Processing Service to enable community members to work with a diversity of submitted data until they have a standardized pipeline. These pipelines are not as vetted as standardized pipelines.  
__HCA Analysis Working Group (AWG):__ A group of domain specialists that advise on and monitor scientific choices made in the Human Cell Atlas Project.  
__HCA DCP Governance Group:__ A working group of the Human Cell Atlas project that advises the DCP, assuring alignment of DCP development efforts with the scientific project it supports.  
__Onboarding:__ The process of taking an existing pipeline and engineering it to work in the HCA DCP.  
__Scoping:__ The process of investigating the available resources and knowledge in a community around a specific assay to better understand how much work it would take to make a pipeline to process data from that assay.  
__Standardized Pipeline:__ An official pipeline of the HCA project.  
__Transformative Analysis:__ The processing of data that changes data or creates new data. This would not include processes that solely subset or reorganize data.

## In-scope

### Pipeline Creation

* Scoping or review of scoping for new pipelines  
* Suggesting, but not selection of, new standardized pipelines for the HCA DCP  
* Creating all standardized HCA pipelines and documenting them  
* Reviewing, on-boarding, and potentially creating community pipelines  
* Reviewing community tools for use in data processing pipelines  
* Working with the community to provide feedback on metadata standards for raw data in the HCA DCP to provide the required information to enable processing of the raw data  
* Evaluating and adopting file formats and associated standards for outputs of pipelines, ensuring the data is accessible with standard tools and interoperable with downstream processes  

### Pipeline Benchmarking

* Investigating and identifying data for pipeline benchmarking  
* Establishing and operating automated practices around scientific testing of pipelines  
* Establishing standard benchmarking used to evaluate data processing pipelines  

### Pipeline Improvement

* Updating pipelines ran in the HCA DCP for optimization of cost, time, or scientific benchmarks  

### Support

* Performing outreach to users on the usability of data in downstream applications  
* Performing outreach to developers on pipeline development in the HCA DCP  
* Working with the metadata team and data consumers to ensure the metadata provided about pipelines and the analysis results meet the needs of downstream users  
* Creating tools that support the creation of community pipelines - may include software, documentation or support  

## Scientific "guardrails"

* Initial versions of standardized pipelines must be reviewed by the AWG.  
* Updates to standardized pipelines that change the scientific output must be reviewed by the AWG. Engineering updates that do not change the scientific output do not require review by the AWG.  
* Selection of standardized pipelines to develop must be approved by the HCA DCP Governance Group (Note, this does not apply to Community Pipelines which are onboarded as a service to the scientific community). The Data Processing Pipelines team is responsible for setting the development priorities for the selected pipelines.  

## Out of Scope

* Educational training or user assistance on how to perform data analysis with outputs of pipelines  
* Recommendations or demonstrations of downstream tool use beyond initial data loading  
* Novel methodological development is not performed by the Data Processing Pipelines team unless it is essential to the pipeline, it is not available in community tools, and requires minimal effort to implement  
* The Data Processing Piplines team preferences using 3rd party software from the community but will not maintain or perform long-term engineering for those tools.  

## Milestones and Deliverables

__2018:__ Two data processing pipelines in production (Smart-seq2 and 10xv2 human data)  
__2019:__ Adding at least three more data processing pipelines, including 10xv2 snRNA-seq and mouse pipelines for all current analysis pipelines Aim to have at least one _in situ_ transcriptomics pipeline in this year. Aim for one of these pipelines to be a community pipeline. Improved automated testing and benchmarking of analysis pipelines  

## Roles

### Project Lead

[Timothy Tickle](mailto:ttickle@broadinstitute.org)

### Product Owner

[Kylee Degatano](mailto:kdegatano@broadinstitute.org)

### Technical Lead

[Rex Wang](mailto:chengche@broadinstitute.org)  
[Saman Ehsan](mailto:sehsan@broadinstitute.org)  

## Communication

### Mailing List

[Team email](mailto:pipelines-team@data.humancellatlas.org): pipelines-team@data.humancellatlas.org  

### Slack Channels

__HumanCellAtlas#mintteam:__ team discussions, a place to directly communicate to the team, who works on the service and the pipelines, as needed  
__HumanCellAtlas#secondary-analysis:__ community-focused space for conversations around data processing pipelines that run in the DCP  

## Github repositories

[scTools](https://github.com/HumanCellAtlas/sctools): Tools for single cell data processing  
[Skylab Analysis](https://github.com/HumanCellAtlas/skylab-analysis): Analysis and benchmarking reports for Skylab pipelines  
[Skylab](https://github.com/HumanCellAtlas/skylab): HCA data processing pipeline  
