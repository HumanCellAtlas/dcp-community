
# Data Store


## Description
The Data Store is a scientific data sharing/publishing/distribution framework, providing file/bundle management on multiple clouds at petabyte-scale. It defines public APIs for storage, retrieval, and subscription to events that functions transparently across multiple cloud systems such as AWS and GCP.

## Objective
The objective of the Data Store group is to deliver substantively complete functionality on all of the in-scope items listed in this charter.

## In-scope

### Interfaces
* Data Store read and write APIs for data (PUT bundle, PUT file, GET bundle, GET file) - maintenance and extension of the implementation of the basic data access APIs.
* Maintain and extend the **Checkout service API** which enables data checkout to a local filesystem or a personal cloud environment
* Maintain and extend the **Collections service API** to do basic operations on arbitrary collections of objects in the Data Store.
* Publish API documentation and examples for both the Data Store REST interface and Python bindings.
* The **Command Line Interface** (CLI) is a foundational tool for interacting with the DCP. The Data Store team is responsible for the specific Data Store commands and the maintenance of the infrastructure that allows other services such as Upload and Ingest to integrate their commands into the CLI.

### Core capabilities
* Maintain and extend the DSS data model and lifecycle (such as versioned bundles)
* Transition Data Store Subscriptions/Eventing services from the current dependence on Elastic Search Percolate to the AWS and GCP cloud infrastructure.
* Multi-cloud replication of objects
   1. Maintenance and improvements to the synchronization implementation between AWS and GCP
   2. Document interfaces to enable new cloud implementations by 3rd parties 
   3. Supporting multiple replicas within a single cloud
* Support for pluggable indexes - Define a standard interface to enable pluggable indexing modules to receive Data Store events

### Security
* User authentication system implementation
* Data access authorization system implementation 
* DevSecOps - implementation of features required for eventual FISMA moderate deployments (authentication, authorization, logging, auditing, etc).
* Operations for Data Store - Implement and configure tools to facilitate the operation of the Data Store service in a production environment

### Community engagement
* Triage and integration of feature requests from the community into the Data Store roadmap. 
* Review and acceptance process for third party software contributions through pull requests
* Outreach and engagement of the community
* Training
* Hackathons

## Out-of-scope
* Other index/query methods/engines - we should implement these as stand-alone projects against modular index/query API.
* Matrix service API

## Milestones
* Mid-2018: 1000 bundle test scale, deploy as part of HCA DCP Pilot
* EOY 2018: add checkout, collections, improved scaling/hardening, generic events to support stand-alone indexers, additional gaps identified in HCA DCP Pilot.

## Roles

### Project Lead 
[Brian O’Connor](mailto:brocono@ucsc.edu) 

### Product Owner 
[Kevin Osborn](mailto:kosborn2@ucsc.edu) 

### Technical Lead 
[Hannes Schmidt](mailto:hannes@ucsc.edu) 

## Communication
### Slack Channels
* HumanCellAtlas/data-store: general data store discussions
* HumanCellAtlas/data-store-eng: development discussions

## Github repositories
* https://github.com/HumanCellAtlas/data-store
* https://github.com/HumanCellAtlas/dcp-cli
* https://github.com/HumanCellAtlas/metadata-api
* https://github.com/chanzuckerberg/cloud-blobstore
* https://github.com/HumanCellAtlas/checksumming_io

