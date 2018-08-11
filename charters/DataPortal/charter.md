# Data Portal


## Description
The Data Portal provides the overall web-site for the HCA DCP with entry point from https://www.humancellatlas.org/. It provides the user experience for scientific and general users to upload, explore, search and run analyses on human genomic data. The Data Browser ("Explore") application provides a faceted search capability to find and download subsets of this data.

## Objective
Implementation of the Data Coordination Platform (DCP) website experience. Provide the access point for scientific and general users to search all HCA genetic data in order to identify a subset of the data for download and/or further analysis, providing file/bundle search on multiple clouds at PB scale, with strong public APIs.

## In-scope
### General
* UX, visual and branding design with input from CZI and the GG
* Production operation of the overall Data Portal web-site (see exclusions in Out-of-scope section)
* Implementation performance scaling
* DevSecOps compliance as required by the HCA Security Team
* DevSecOps features for FISMA moderate deployments (authZ/N, logging, auditing, etc) by leveraging other projects (if this becomes a future requirement)
* Community engagement
   * Triage and integration of feature requests into the work queue 
   * Support plan for escalation of user reported bugs
### Data Portal
* Select and deploy content management system
* Integrate Data Browser and Data Portal UIs into a seamless UX and provide link to other groups' applications (e.g. Ingest UI)
* Frequent updates of summary statistics (cells, organs, donors, projects, labs, etc.) on Home Page
* Coordinate development of user guide content (e.g. “Learn”) from multiple contributors
* Coordinate development of developer documentation content (e.g. “Develop”) from multiple contributors. Includes API documentation and development guides.
* Click through to Data Browser from search box and human body image
* Registration form for tertiary portals and list of portals available with click through to hosting web site (e.g. Dockstore)
* Integration with Zendesk help desk
* Integration with Vanilla Forums (discussion forum for developers)
* Integration with Canny (to capture user feedback)
### Data Browser
* Create (or use Data Store provided) indexes as necessary for files in the Data Store, using public DSS APIs to read the metadata
* Browser support for multiple metadata schemas
* Search views of Specimens, Projects and Files
* Browser UI to search the DSS by specifying multiple search criteria (“facets”)
* Download selected files from the DSS via the UI and/or HCA cli (the HCA cli itself is out-of-scope for this charter)
* AuthN for tracking users' collections, saved searches, etc.
* Full authN/Z if the HCA stores controlled access data in the future. Leverage AuthN/Z from Data Store.
* Browser interface with DSS Collections service to save/view/list/share/delete the query results from a search operation
* Handoff of a Collection to tertiary portals. Expand the current prototype for handoff to FireCloud.
* Save/view/share/update/delete search queries
* Search results handoff to Matrix service
* API Documentation for portable, cloud-neutral APIs

## Out-of-scope
* Production operation of other HCA Data Coordination Platform (DCP) components in AWS (e.g. Data Store, Ingest, Secondary Analysis, Tertiary Analysis)
* Contribute/Ingest UI (from EBI)
* Methods Registry providing manage/search/filter/sort visualization and methods packages in the tertiary portal (from Broad)
* Primary interface for collecting user feedback and feature/bug requests

## Milestones
* Oct 2018:  Deploy MVP feature set as part of HCA DCP Pilot phase
* Nov 2018:  Present Pilot phase portal to community
* EOY 2018:  Improved scaling/hardening for additional gaps identified in HCA DCP Pilot phase

## Roles

### Project Lead
[Brian O’Connor](mailto:brocono@ucsc.edu) 

### Product Owner
[Trevor Heathorn](mailto:theathor@ucsc.edu) 

### Technical Lead
Portal: [Dave Rogers](mailto:dave@clevercanary.com)
Browser: [Hannes Schmidt](mailto:hannes@ucsc.edu)

## Communication

### Slack Channel(s)
* HumanCellAtlas/data-portal : general data portal discussions
* HumanCellAtlas/data-portal-dev : data portal development discussions
* HumanCellAtlas/content-team : data portal content development discussions
* HumanCellAtlas/data-browser-dev : data browser development discussions
### Mailing list(s)
### Discussion Forum(s)

## Github repositories
* https://github.com/HumanCellAtlas/data-portal
* https://github.com/HumanCellAtlas/data-portal-content
* https://github.com/HumanCellAtlas/data-browser
* https://github.com/DataBiosphere/azul
