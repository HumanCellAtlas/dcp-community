# Data Portal


## Description
The Data Portal is a landing site for the Human Cell Atlas (HCA) Data Coordination Platform (DCP). It is hosted at https://www.data.humancellatlas.org/ with an access link from the main HCA website at https://www.humancellatlas.org/. It provides the user experience for scientific and general users to upload, explore, search, and run analyses on human genomic data. The Data Browser ("Explore") application provides a faceted search capability to find and download subsets of this data.

## Objective
Implementation of the DCP website experience. Provide the access point for scientific and general users to search all HCA genetic data in order to identify a subset of the data for download and/or further analysis, providing file/bundle search on multiple clouds at petabyte scale with public APIs.

## In-scope
### General
* UX, visual, and branding design with input from the HCA brand working group
* Adhere to the HCA DCP community best practices for Continuous Integration (CI), Continuous Delivery (CD), system logging via Google Analytics, etc.
* Production operation of the overall Data Portal website in Amazon Web Services (AWS), with exclusions listed in the out-of-scope section
* Implementation performance scaling
* DevSecOps compliance as required by the HCA Security Team
* Community engagement
   * Triage and integration of feature requests into the work queue 
   * Support plan for escalation of user reported bugs
### Data Portal
* Select and deploy a content management system
* Integrate Data Browser and Data Portal UIs into a seamless UX and provide link to other groups' applications (e.g. a Data Ingestion UI)
* Frequent updates of summary statistics (cells, organs, donors, projects, labs, etc.) on the Home Page
* Coordinate development of user guide content (e.g. “Learn”) from multiple contributors
* Coordinate development of developer documentation content (e.g. “Develop”) from multiple contributors. Includes API documentation and development guides.
* Click through to Data Browser from search box and human body image
* Registration form for tertiary portals and list of portals available with click through to hosting web site (e.g. Dockstore)
* Integration with Zendesk help desk
* Integration with Vanilla Forums (discussion forum for developers)
* Integration with Canny (to capture user feedback)
### Data Browser
* Create (or use Data Storage Service [DSS] provided) indexes to access files in the Data Store by using public DSS APIs to read the metadata
* Browser support for multiple metadata schemas that are supported by the schema access library maintained by the Ingest service
* Search views of Specimens, Projects and Files
* Browser UI to search the DSS by specifying multiple search criteria (“facets”)
* Download selected files from the DSS via the UI and/or HCA DCP Command Line Interface (CLI)
* Leverage the authentication and authorization implementation from the Data Store
* Browser interface to the DSS Collections service to save, view, list, share and delete the query results from a search operation
* Handoff of a Collection of search results to a tertiary portal. Provide for direct handoff of search results to FireCloud.
* Facilities to save, view, share, update, and delete search queries and personal collections on a per-user basis
* Search results handoff to the Matrix service
* API Documentation for portable, cloud-neutral APIs

## Out-of-scope
* Production operation of other HCA DCP components in AWS (e.g. the DSS, data ingestion process, Secondary Analysis workflows, Tertiary Analysis portals, etc.)
* Any separate data ingest application developed by The European Bioinformatics Institute (EBI) which may replace or augment the "Contribute" tab in the Data Portal
* Methods Registry providing management, searching, filtering, sorting, visualization, and methods packages in the tertiary portal (registry is developed and maintained by the Broad Institute)
* The HCA DCP CLI infrastructure implementation (which is part of the DSS)
* Primary interface for collecting user feedback and feature/bug requests

## Milestones
* Oct 2018:  Deploy MVP feature set as part of HCA DCP Pilot phase
* Nov 2018:  Present Pilot phase portal to community
* EOY 2018:  Improved scaling/hardening for additional gaps identified in HCA DCP Pilot phase
* 1H  2019:  Save search queries and query results (using DSS Collections APIs). Handoff of a Collection of search results to the Matrix service and to tertiary portals. Vanilla Forums support. Canny support.

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
