
# Data Store


## Description
The Data Store is a scientific data sharing/publishing/distribution framework, providing file/bundle management on multiple clouds at PB scale, with strong public APIs. It provides a simple API for storage, retrieval, and subscription to events that functions transparently across multiple cloud systems such as AWS and GCP.

## Objective
The objective of the Data Store group is to deliver substantively complete functionality on all of the in-scope items listed in this charter.

## In-scope

### Interfaces
* DSS data read and write API (PUT bundle, PUT file, GET bundle, GET file) - maintenance and extension of the implementation of the basic data access APIs.
* Checkout service APIs - Provide continuing support for the ability to checkout the data to a local filesystem, or a personal cloud environment
* Collections service APIs - Maintenance and extension of the ability to do basic operations on arbitrary collections of objects in the Data Store.
* API Documentation - Programmatic APIs available for the Data Store include the REST interface and the Python bindings. Documentation and examples will be created for both of these APIs.
* HCA DCP CLI tool - The HCA DCP CLI is a foundational tool for the DCP and its users. All subcomponents in the DCP use the same CLI system. The Data Store team will maintain the infrastructure to support the general CLI architecture as well as the CLI commands relating to the Data Store itself. Other modules such as Upload and Ingest will be responsible for implementing their respective functional components of the CLI

### Core capabilities
* DSS data model and lifecycle (versioned bundles, etc) - Ongoing support and maintenance of the implementation of the data model. 
* Subscriptions/Eventing - Implementation of Data lifecycle web-hooks (new bundle, new file, delete bundle, delete file). The Data Store implementation will move away from the current dependance on Elastic Search Percolate for our event subsystem. Eventing will depend instead on the AWS and GCP cloud infrastructure directly.
* Multi-cloud replication of objects - There are three parts to this:
   1. Maintenance and improvements to the synchronization implementation between AWS and GCP
   2. Extending the cloud support to more vendors (such as Microsoft Azure) 
   3. Supporting multiple replicas within a single cloud.
* Support for plug-able indexes - Provide a standard interface for connecting indexing modules to the Data Store. This interface will provide a mechanism to connect indexing subsystems to receive events about the data. 

### Security
* User authentication system implementation
* Data access authorization system implementation 
* DevSecOps - implementation of features required for eventual FISMA moderate deployments (authentication, authorization, logging, auditing, etc).
* Operations for DSS - Implement and configure tools to facilitate the operation of the Data Store service in a production environment

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
* Mid-2018:  1000 bundle test scale, deploy as part of HCA DCP Pilot
* EOY 2018: add checkout, collections, improved scaling/hardening, generic events to support stand-alone indexers, additional gaps identified in HCA DCP Pilot.
* Future: (not in order of precedence) native GCP support, Authorization support for controlled-access data, additional scale/hardening, Biosphere requirements, tiered storage, content zones, FISMA moderate capabilities, single-replica deployments

## Roles

### Project Lead 
[Brian O’Connor](mailto:brocono@ucsc.edu) 

### Product Owner 
[Kevin Osborn](mailto:kosborn2@ucsc.edu) 

### Technical Lead 
[Hannes Schmidt](mailto:hannes@ucsc.edu) 

## Communication
### Slack Channels
* HumanCellAtlas/data-store : general data store discussions
* HumanCellAtlas/data-store-eng : development discussions
### Mailing list(s)
### Discussion Forum(s)

## Github repositories
* https://github.com/HumanCellAtlas/data-store
* https://github.com/HumanCellAtlas/dcp-cli
* https://github.com/HumanCellAtlas/metadata-api
* https://github.com/chanzuckerberg/cloud-blobstore
* https://github.com/HumanCellAtlas/checksumming_io

