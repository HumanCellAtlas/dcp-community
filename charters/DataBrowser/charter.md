# Data Browser


## Description
The Data Browser (available via the Data Portal 'Explore' section) is an application that provides a faceted search capability allowing users to search and download subsets of cellular resolution data hosted by the Data Coordination Platform (DCP) and facilities to pass such subsets of data to the Expression Matrix Service and Analysis Applications.

## Definitions
*DCP* The Data Coordination Platform is the name given to the entire system used to ingest, validate, store, analyze, and make available the data in the Human Cell Atlas project.

*Data Portal* The landing site for the Human Cell Atlas (HCA) DCP.

*Analysis Applications* Third party portals listed in the 'Analyze' section of the Data Portal UI.

## Objectives
The objective of the Data Browser is to provide an easy-to-use User Interface (UI) and RESTful API allowing users to search, download and analyze data stored in the DCP at petabyte scale on public clouds.

## In-scope

### Core capabilities
* Create indexes to access files in the Data Store by using public Data Store Service (DSS) APIs to read the metadata.
* Support multiple metadata schemas via a shared schema access library.
* Provide multiple search views of the DCP data such as by Projects, Files and Specimens.
* Support for specifying multiple search criteria ('facets') through the UI.
* Provide for download of user-selected files from the Data Store via the UI and/or HCA DCP Command Line Interface (CLI).
* Provide facilities to save, view, share, update, and delete search queries on a per-user basis.
* Provide facilities to utilize the Date Store Collections Service to save, view, list, share and delete the query results from a search query.
* Provide a facility to pass search results to the Expression Matrix Service.
* Provide a facility to pass search results to Analysis Applications.
* API documentation for portable, cloud-neutral RESTful APIs for faceted search of DCP data.

### Security
* Leverage the authentication and authorization implementation libraries from the DevSecOps team to implement user authentication and access control for the Data Browser.
* Implement and configure tools to facilitate the operation of the Data Browser in a production environment.
* Provide day-to-day operation of the Data Browser production deployment including but not limited to monitoring of errors and health of intrinsic components, fielding help requests, monitoring security, and assisting with the roll-out of new data into the deployment.

### Community engagement
* Provide triage of feature requests from the community and integration of such requests into the Data Browser roadmap.
* Outreach to, and engagement of, the DCP community on the usability of the APIs and the UI.

## Out-of-scope
* Implementation of other language bindings for the APIs.

## Milestones and Deliverables
* Nov 2018:  Deploy a minimum viable product (MVP) feature set as part of the HCA DCP beta test phase. Interface to the Expression Matrix Service.
* Q1  2019:  Implement as needed improved scaling/hardening to address any gaps identified during the HCA DCP beta test phase.
* 1H  2019:  Save search queries. Pass search results to Analysis Applications. Integration with authentication and authorization facilities. API documentation.

## Roles

### Project Lead
[Brian O’Connor](mailto:brocono@ucsc.edu) 

### Product Owner
[Trevor Heathorn](mailto:theathor@ucsc.edu) 

### Technical Lead
[Hannes Schmidt](mailto:hannes@ucsc.edu)

## Communication

### Slack Channels
* [HumanCellAtlas/data-browser](https://humancellatlas.slack.com/messages/data-browser): data browser development discussions

## Github repositories
* https://github.com/HumanCellAtlas/data-browser - The repository for the UI implementation. Use it to report issues and request new features for the Data Browser.
* https://github.com/DataBiosphere/azul - The repository for the Metadata Indexer engine and Web Service RESTful API implementation.
* https://github.com/HumanCellAtlas/metadata-api - A light-weight wrapper library around the JSON metadata in HCA data bundles.
