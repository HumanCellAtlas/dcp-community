
# Data Store


## Description
The Data Store is a scientific data sharing/publishing/distribution framework, providing file/bundle management on multiple clouds at petabyte-scale. It defines public APIs for storage, retrieval, and subscription to events that functions transparently across multiple cloud systems such as Amazon Web Service and Google Cloud Platform. The Data Store is designed to be reused in multiple projects, with HCA being the first user. 

## Definitions
**Bundle** A bundle is a list of related files along with some very basic metadata such as filenames.

**DCP** The Data Coordination Platform is the name given to the entire system used to ingest, validate, store, analyzes, and make available the data in the Human Cell Atlas project.

## Objectives
The objective of the Data Store group is to deliver a versioned immutable object based data repository that is highly available and scalable. Data will be replicated to at least two commercial clouds (Amazon and Google). Data will be accessible through a variety of programmatic interfaces as well as a command line interface. 

## In-scope

### Interfaces
* Data Store read and write APIs for data and metadata - maintenance and extension of the implementation of the basic data access APIs. There are two public APIs available, the **REST API** and the **Python bindings**.
* Maintain and extend the **Checkout service API** which enables data copy to a local filesystem or a personal cloud environment.
* Maintain and extend the **Collections service API** to do basic operations on arbitrary collections of objects in the Data Store.
* Publish API documentation and examples for both the Data Store REST interface and Python bindings.
* The **Command Line Interface** (CLI) is a foundational tool for interacting with the DCP. The Data Store team is responsible for the specific Data Store commands and the maintenance of the infrastructure that allows other services such as Upload and Ingest to integrate their commands into the CLI.

### Core capabilities
* Maintain and extend the Data Store data model and data lifecycle. The data model is represented by bundles and files of arbitrary information. The Data Store team will own the process of collecting new bundle requirements from users and assessing them against the datastore design before sending them to the metadata team for the definition of a new spec. The Data Store owns the bundle use cases and bundle type definitions, while the precise specifications will be negotiated between the Metadata and other teams.
* Support for reliable Subscriptions/Eventing services
* Multi-cloud replication of objects
   1. Maintenance and improvements to the synchronization implementation between AWS and GCP
   2. Document interfaces to enable new cloud implementations by 3rd parties 
   3. Supporting multiple replicas within a single cloud
* Support for pluggable indexes - Define a standard interface to enable pluggable indexing modules to receive Data Store events

### Security
* User authentication system implementation for the Data Store
* Data access authorization system implementation for the Data Store
* DevSecOps - implementation of features required to the core Data Store code to support FISMA moderate capabilities in community reuse code bases (authentication, authorization, logging, auditing, etc). The authentication and authorization system will support all rights of data subjects as defined in [GDPR](https://gdpr-info.eu/)
* Operations for Data Store - Implement and configure tools to facilitate the operation of the Data Store service in a production environment

### Community engagement
* Triage and integration of feature requests from the community into the Data Store roadmap. 
* Review and acceptance process for third party software contributions through pull requests
* Outreach and engagement of the community on use/usability of the APIs
* Provide collaboration with groups to explore what it would take to implement reuse of the Data Store.
* Host hackathons for extending the Data Store feature set. 

## Out-of-scope
* Other index/query methods/engines - we should implement these as stand-alone projects against a modular index/query API.
* FISMA moderate certification for the core Data Store code base
* Implementation of other language bindings for the APIs other than Python
* The specification for the format, naming, and content of bundles and files stored in the Data Store.

## Milestones and Deliverables
* Mid-2018: 1000 bundle test scale, deploy as part of HCA DCP Pilot
* EOY 2018: add checkout, collections, improved scaling/hardening, generic events to support stand-alone indexers, additional gaps identified in HCA DCP Pilot.
* First half of 2019: Document Data Store interfaces so that the community is enabled to deploy storage on a configurable cloud (AWS or GCP) with the system logic still running in AWS. Also document replication APIs to enable the community to implement new cloud support. 
* First half of 2019: Transition Data Store Subscriptions/Eventing services from the current dependence on Elastic Search Percolate to the AWS and GCP cloud infrastructure.

## Roles

### Project Lead 
[Brian O’Connor](mailto:brocono@ucsc.edu) 

### Product Owner 
[Kevin Osborn](mailto:kosborn2@ucsc.edu) 

### Technical Lead 
[Brian Hannafious](mailto:bhannafi@ucsc.edu) 

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

