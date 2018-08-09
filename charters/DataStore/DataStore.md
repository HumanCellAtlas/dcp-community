
# Data Store


## Description
The Data Store is a scientific data sharing/publishing/distribution framework, providing file/bundle management on multiple clouds at PB scale, with strong public APIs. It provides a simple API for storage, retreival, and subscription to events that functions transpareently across multiple cloud systems such as AWS and GCP.

## Objective
The objective of the Data Store group is to deliver substantively complete functionality on all of the in-scope items listed in this charter.

## In-scope
* DSS data read and write API (PUT bundle, PUT file, GET bundle, GET file)
* DSS data model and lifecycle (versioned bundles, etc)
* Data lifecycle webhooks (new bundle, new file, delete bundle, delete file)
* Checkout service - Provide continueing support for the ability to checkout the data to a local filesyste, or a personal cloud environment
* Collections service - Maintenance and extentison of tthe ability to form arbitrary collections of objects in the Data Store
* Multi-cloud replication of objects - This will entail extending the cloud support to more vendors (such as Microsoft Azure) as well as supporting multiple replicas within a single cloud.
* Support for plugable indexes - Provide a standard interface for indexing modules. The plug-in interface will interact with the eventing subsystem so that all indexing systems will be notified of important data store events.This will allow a variety of 
* Data access authorization (auth{n,z} for limited access data)
* HCA DCP CLI tool
* Implementation performance scaling
* Generic events
* API Documentation for above APIs
* Portable, cloud-neutral APIs; portable implementation of DSS on multiple clouds.
* DevSecOps features for eventual FISMA moderate deployments (authZ/N, logging, auditing, etc).
* Community engagement
   * Triage and integration of feature requests into work queue 
   * Maintenance of outreach and engagement 
* Operations for DSS

## Out-of-scope
* Other index/query methods/engines - we should implement these as stand-alone projects against modular index/query API.
* Matrix service API

## Milestones
* Mid-2018:  1000 bundle test scale, deploy as part of HCA DCP Pilot
* EOY 2018: add checkout, collections, improved scaling/hardening, generic events to support stand-alone indexers, additional gaps identified in HCA DCP Pilot.
* Future: native GCP support, AuthZ support for controlled-access data, additional scale/hardening, Biosphere requirements, tiered storage, content zones, FISMA moderate capabilities, single-replica deployments, et

## Roles

### Project Lead [Brian O’Connor](mailto:brocono@ucsc.edu) 

### Product Owner [Kevin Osborn](mailto:kosborn2@ucsc.edu) 

### Technical Lead [Hannes Schmidt](mailto:hannes@ucsc.edu 

## Communication
* HumanCellAtlas/data-store : general data store discussions
* HumanCellAtlas/data-store-eng : development discussions

## Github repositories
* https://github.com/HumanCellAtlas/data-store
* https://github.com/HumanCellAtlas/dcp-cli
* https://github.com/HumanCellAtlas/metadata-api
* https://github.com/chanzuckerberg/cloud-blobstore
* https://github.com/HumanCellAtlas/checksumming_io

