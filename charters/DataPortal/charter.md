# Data Portal


## Description
The Data Portal is a landing site for the Human Cell Atlas (HCA) Data Coordination Platform (DCP). It is hosted at https://www.data.humancellatlas.org/ with an access link from the main HCA website at https://www.humancellatlas.org/. It provides the user experience for scientific and general users to upload, explore, search, and run analyses on human genomic data. The Data Browser ("Explore") application provides a faceted search capability to find and download subsets of this data.

## Objective
The objective of the Data Portal is to provide the DCP website implementation. It provides the access point for scientific and general users to search all HCA genetic data in order to identify a subset of the data for download and/or further analysis, and file/bundle search with public APIs at petabyte scale on public clouds.

## In-scope
### General
* UX, visual, and branding design with input from the HCA brand working group
* Adhere to the HCA DCP community best practices for Continuous Integration (CI), Continuous Delivery (CD), and system logging via Google Analytics, etc.
* Production operation of the overall Data Portal website in Amazon Web Services (AWS), with exclusions listed in the out-of-scope section
* Meet the performance scaling objectives of the DCP
* DevSecOps compliance as required by the HCA Security Team
* Leverage the authentication and authorization implementation libraries from the DevSecOps team to implement login and access control for the Data Portal and Data Browser
* Community engagement:
   * Triage and integration of feature requests into the work queue 
   * Support plan for escalation of user reported issues
### Data Portal
* Select and deploy a content management system
* Integrate Data Browser and Data Portal UIs into a seamless UX and provide links to other groups' applications (e.g. the Ingestion Service UI)
* Frequent updates of summary statistics (cells, organs, donors, projects, labs, etc.) on the DCP home page
* Coordinate development of user guide content (e.g. “Learn”) from multiple contributors
* Coordinate development of developer documentation content (e.g. “Develop”) from multiple contributors. Includes API documentation and development guides.
* Click through to Data Browser from search box and human body image
* Registration form for tertiary portals and list of portals available with click through to hosting web site (e.g. Dockstore)
* Integration with help desk, discussion forum, and user feedback applications
### Data Browser
* Create (or use Data Storage Service [DSS] provided) indexes to access files in the Data Store by using public DSS APIs to read the metadata
* Browser support for multiple metadata schemas that are supported by the schema access library maintained by the Ingestion Service
* Search views of Specimens, Projects and Files
* Support for specifying multiple search criteria (“facets”) through the Browser UI
* Download of selected files from the Data Store via the UI and/or HCA DCP Command Line Interface (CLI)
* Browser interface to the Date Store Collections Service to save, view, list, share and delete the query results from a search operation
* Search results / Collections handoff to Analysis Applications. Provide for direct handoff of search results to FireCloud.
* Facilities to save, view, share, update, and delete search queries and personal Collections on a per-user basis
* Search results / Collections handoff to the Matrix service
* API Documentation for portable, cloud-neutral APIs

## Out-of-scope
* Production operation of applications and services linked to from the Data Portal (e.g. the Data Storage Service, Ingestion Infrastructure, Upload Service, Data Processing Pipelines, Analysis Applications, etc.)
* Any separate Ingestion Service application which may replace or augment the "Contribute" tab of the Data Portal
* Methods Registry providing management, searching, filtering, sorting, visualization, and methods packages in the "Analyze" tab of the Data Portal
* Primary interface for collecting user feedback and feature/bug requests

## Milestones
* Oct 2018:  Deploy a minimum viable product (MVP) feature set as part of HCA DCP beta test phase
* Nov 2018:  Present beta phase portal to the DCP community
* EOY 2018:  Implement as needed improved scaling/hardening to address any gaps identified during the HCA DCP beta test phase
* 1H  2019:  Save search queries and query results (using DSS Collections APIs). Search results / Collections handoff to the Matrix Service and to Analysis Applications. Discussion forums support. User feedback application support.

## Roles

### Project Lead
[Brian O’Connor](mailto:brocono@ucsc.edu) 

### Product Owner
[Trevor Heathorn](mailto:theathor@ucsc.edu) 

### Technical Lead
Portal: [Dave Rogers](mailto:dave@clevercanary.com)
Browser: [Hannes Schmidt](mailto:hannes@ucsc.edu)

## Communication

### Slack Channels
* HumanCellAtlas/data-portal: general data portal discussions
* HumanCellAtlas/data-portal-dev: data portal development discussions
* HumanCellAtlas/content-team: data portal content development discussions
* HumanCellAtlas/data-browser-dev: data browser development discussions

## Github repositories
* https://github.com/HumanCellAtlas/data-portal
* https://github.com/HumanCellAtlas/data-portal-content
* https://github.com/HumanCellAtlas/data-browser
* https://github.com/DataBiosphere/azul
