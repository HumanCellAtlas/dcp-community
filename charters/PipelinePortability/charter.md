# [Pipeline Portability Service](mailto:pipelines-team@data.humancellatlas.org)

## Description

Reproducibility of analysis by others is a cornerstone of science. To satisfy this need, HCA data processing pipelines should be able to be executed by others in other projects and infrastructure. The Portability Service confirms that pipelines developed for the HCA DCP can be successfully executed outside of the HCA DCP. Confirmation that HCA data processing pipelines can execute internally in the DCP is not assessed by this service but such confirmation is a part of our standard development and testing practices.   

## Objectives

To confirm there are no technical barriers to executing pipelines outside of the DCP infrastructure, the Portability Service executes pipelines in a set of other environments and verifies that they run successfully. The Portability Service implementation team as well as third parties can submit environments to be included in the Portability Service's test set.   

## Definitions

__3rd party:__ A developer outside of the team implementing the portability service.  
__3rd party backend:__ An execution environment or storage platform that could be used to execute data processing pipelines but is not officially used or supported by the DCP.  
__HCA DCP:__ The Human Cell Atlas Data Coordination Platform is the data center for the HCA.  

## In-scope

* Programmatic authorized access to submit pipelines (to be automatically executed in all integrated backends)  
* Programmatic authorized access to extend the backend of the Portability Service  
* A UI to describe the success of pipeline execution on different backends  
* Documentation to describe how to submit a backend or pipeline to the system  
* Documentation on portability best practices that are learned as the system is used  
* Incorporating backends explicitly used by the infrastructure of the HCA DCP (ie. testing platforms used by the data storage service, execution environments used by the data processing service, and workflows languages officially supported by the project)  

## Scientific "guardrails"

Portability service will not assess the scientific accuracy of pipelines, and only report on successful execution.  

## Out-of-scope

* Educational training or user assistance on how to create pipelines  
* Updating backends so they support pipeline execution  
* Updating pipelines so they are portable to environments  
* Establishing standard data sets for portability (they are provided per pipeline)  
* Adding 3rd party backends  

## Milestones and Deliverables

__2018:__ More than one backend are integrated and documentation is available for 3rd parties  
__2018:__ Authenticated access is established for an endpoint to submit pipelines  
__2019:__ The Portability Service will enter temporary maintenance mode which will be re-evaluated at the end of the year  

## Roles

### Project Lead

[Timothy Tickle](mailto:ttickle@broadinstitute.org)

### Product Owner

[Kylee Degatano](mailto:kdegatano@broadinstitute.org)

### Technical Lead

[Marcus Kinsella](mailto:mkinsella@chanzuckerberg.com)

## Communication

### Mailing list

Team email: pipelines-team@data.humancellatlas.org

### Slack Channels

* [HumanCellAtlas/portability](https://humancellatlas.slack.com/messages/portability): To discuss portability of HCA data processing pipelines  

## Github repositories

[Portability](https://github.com/HumanCellAtlas/portability): Source code for the portability service.
