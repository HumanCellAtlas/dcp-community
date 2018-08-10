# Data Portal


## Description
The Data Portal provides the overall web-site for the HCA DCP with entry point from https://www.humancellatlas.org/. It provides the user experience for scientific and general users to upload, explore, search and run analyses on human genomic data. The Data Browser ("Explore") application provides a faceted search capability to find and download subsets of this data.

## Objective
Implementation of the Data Coordination Platform (DCP) website experience. Provide the access point for scientific and general users to search all HCA genetic data in order to identify a subset of the data for download and/or further analysis, providing file/bundle search on multiple clouds at PB scale, with strong public APIs.

## In-scope
* Select and deploy content management system
* UX, visual and branding design with input from CZI
* Integrate multiple UIs (Ingest, Data Browser and Data Portal) into a seamless UX
* Frequent updates of summary statistics (cells, organs, donors, projects, labs, etc.) on Home Page
* Coordinate development of user guide content (e.g. “Learn”) from multiple contributors
* Coordinate development of developer documentation content (e.g. “Develop”) from multiple contributors. Includes API documentation and development guides.
* Click through to Data Browser from search box and human body image
* Registration form for tertiary portals and list of portals available with click through to hosting web site (e.g. Dockstore)
* Integration with Zendesk help desk
* Integration with Vanilla Forums (discussion forum for developers)
* Integration with Canny (to capture user feedback)
* Production operation of the overall Data Portal web-site, excluding Ingest and Secondary Analysis
* Create multiple Indexes for files in the Data Store using public DSS APIs to read the metadata
* Portable implementation of Browser/Indexer to support future deployment on multiple clouds.
* Browser support for multiple metadata schemas
* Search views of Specimens, Projects and Files
* Browser UI to search the DSS by specifying multiple search criteria (“facets”)
* Download selected files from the DSS via the UI and/or HCA cli.
* Data access authorization (auth{n,z} for limited access data)
* Browser interface with DSS Collections service to save/view/list/share/delete the query results from a search operation
* Handoff of a Collection to tertiary portals
* Save/view/share/update/delete search queries
* Search results handoff to Matrix service
* Implementation performance scaling
* API Documentation for portable, cloud-neutral APIs
* DevSecOps features for eventual FISMA moderate deployments (authZ/N, logging, auditing, etc).
* Community engagement
   * Triage and integration of feature requests into work queue 
   * Maintenance of outreach and engagement


## Out-of-scope
* Production operation of the HCA Data Coordination Platform (DCP) in AWS (CZI)
* Contribute/Ingest UI (from EBI)
* Methods Registry providing manage/search/filter/sort visualization and methods packages in the tertiary portal (from Broad)

## Milestones
* Oct 2018:  deploy MVP feature set as part of HCA DCP Pilot phase
* EOY 2018:  improved scaling/hardening, additional gaps identified in HCA DCP Pilot phase

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
