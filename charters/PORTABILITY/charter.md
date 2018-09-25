# Project Name
Pipeline Portability Service

## Description

The portability service enables the confirmation that pipelines developed for the HCA DCP can be successfully ran outside of the HCA DCP.

## Objectives

The portability service provides the ability to run HCA DCP pipelines 

## In-scope

* Programmatic authorized access to submit pipelines that will be ran to confirm they execute to completion on different computing infrastructure (execution environments)
* Programmatic authorized access to extend the backend of the portability service to include additional execution environments
* A UI to describe the current state of submitted pipelines and execution environments
* Documentation to describe how to submit a backend or pipeline the system
* Documentation on portability best practices

### Scientific "guardrails" [Optional]

Portability service will not comment on scientific accuracy of pipelines, with this constraint it will not need guardrails

## Out-of-scope

* Portability service will not comment on scientific accuracy of pipelines
* Educational training or user assistance on how to create pipelines
* Updating backends so they support pipeline execution
* Updating pipelines so they are portable to environments that do not support workflow language standards
* Establishing standard data sets for portability (they are provided per pipeline)
* Adding 3rd party backends

## Milestones and Deliverables

__2018:__ Authenticated access is established for an endpoint to submit pipelines.   
__2018:__ Basic UI is established for pipeline and backend status.   
__2019:__ Portability service is leveraged in the CI/CD of all pipelines ran by Lira.   
__2019:__ All backends supported by the HCA project are attached.   
__2019:__ Documentation is complete and can be used by 3rd parties.   

## Roles
### Project Lead
[Timothy Tickle](mailto:ttickle@broadinstitute.org)

### Product Owner
[Kylee Degatano](mailto:kdegatano@broadinstitute.org)

### Technical Lead
[Marcus Kinsella](mailto:mkinsella@chanzuckerberg.com)

## Communication

Slack channels
__HumanCellAtlas#portability:__ To discuss portability and access of pipelines to infrastructure outside Lira.

## Github repositories
[Portability](https://github.com/HumanCellAtlas/portability):  Source code for the portability service.
