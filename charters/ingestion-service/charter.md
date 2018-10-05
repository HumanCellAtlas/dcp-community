
# [Ingestion Service](mailto:ingest-team@data.humancellatlas.org)

## Description

The Ingestion Service provides a suite of services to facilitate the contribution of data and metadata to the Data Coordination Platform (DCP). The Ingestion Service allows users to:
  * Upload metadata describing biological materials, data files, and the experimental processes that generated them
  * Validate metadata against endorsed community standards
  * Interact with the DCP Upload Service to track and validate uploaded data files
  * Track submission states that describe the life-cycle of experimental data as it moves through the DCP
  * Generate identifiers for data, metadata and submissions
  * Store submitted data in the DCP Data Storage Service

### Definitions

*Data contributor* - any user who uploads data into the DCP

*Data consumer* - any user who accesses or downloads data from the DCP

*Direct data contributor* - a data contributor who uploads their own data through services offered as part of the DCP in a "self-service" manner. Direct data contributors are also usually the generators of the data

*Data wrangler* - an individual who works on behalf of the Human Cell Atlas (HCA) consortium to provide assistance to data contributors uploading data into the DCP. From an Ingestion Service perspective, data wranglers can themselves be considered as a specialised type of data contributor

*Submission* - the grouping of data and metadata that makes sense for a data contributor to supply as part of a single interaction with the Ingestion Service. For example, a data contributor might typically upload all the data at once for a publication they have in preparation. This grouping does not necessarily correspond to how data is stored or displayed in the DCP and how it should be displayed to a data consumer

*Bundle* - the grouping of data and metadata in the Data Storage Service

## Objectives

The Ingestion Service supports data contributors in the collection, validation and submission of data and metadata at the point of generation.

The primary mission of the Ingestion Service is to define a publicly accessible application programming interface - the **Ingest API** - for the upload of data and metadata that will be stored in the Data Storage Service and that adheres to endorsed community standards.

The secondary mission is to provide convenient, user-friendly methods for the upload of data and metadata to the DCP. The Ingestion Service will provide user interfaces and tools suitable for a variety of data contributor profiles, and will continuously review whether these interfaces and tools adequately serve the needs of the HCA user community. As part of this secondary mission, the Ingestion Service will deliver two main interfaces that utilise the Ingest API to support ingestion of data:

  * A **User Interface** targeting data contributors uploading data and metadata via a form-based user interface
  * A **Spreadsheet Upload** service targeting data generators uploading experimental metadata using spreadsheets in single, canonical defined spreadsheet format based on a specific Excel template (.xlsx) created by the spreadsheet template generator

It is expected that other client interfaces or applications may be developed, either by lab-based data contributors or in some cases by data wranglers to assist those contributors. Such clients will utilise the Ingest API.

The Ingestion Service will additionally deliver tools to support submitters using either the user interface or spreadsheet upload. A **Spreadsheet Template Generator** will target lab-based project managers and will enable the generation of spreadsheet templates suitable for data generators to capture the details of their experiment

## In-scope

### Interfaces

* Develop and maintain a single 'endorsed' user interface that can be used either by data contributors to create, edit and validate submissions
* Provide a user interface that allows data contributors to track the state of their ongoing submissions
* Provide a user interface that allows data contributors to see all submissions they have made to the DCP
* Provide a user interface that allows data contributors to see validation reports about the contents of their submissions (including data file validation reports)
* Provide a user interface that allows data contributors to review and repair any metadata that fails to meet validation criteria, or has been automatically modified (for example, the automatic annotation of data with ontology terms)
* Provide services to communicate to submitters any errors that have encountered during the submission process, irrespective of the method used (e.g. the user interface, a spreadsheet, or some other programmatic client). All submission interfaces must communicate enough information to allow a contributor to review any issues with a submission through the single endorsed user interface, and if possible to take action to repair the erroneous contents
* Develop and maintain services that allow submission of metadata supplied in the form of a canonical spreadsheet format
* Ensure spreadsheet submission services support the submission of spreadsheets that comprehensively capture metadata describing all aspects of an experiment that may be required for downstream analysis, even if this means the metadata *does not currently validate* against endorsed community standards
* Provide services to allow spreadsheets to be re-submitted in order to make updates to an existing submission. These services can place additional restrictions on spreadsheets (for example, they may require the addition of a UUID field in order to identify the target records being updated)

### Core capabilities

* Develop and maintain a RESTful Ingest API supporting the collection of experimental metadata via submissions
* Provide validation services that supply detailed reports, available through this API, on the quality of submitted data with respect to the endorsed community standards delivered by the metadata team
* Provide state tracking services that supply information about the current state of any given submission, allowing submitters to see where in the ingestion process their data is currently
* Provide services for connecting metadata describing experimentally generated data files, and the locations of those data files in the cloud
* Provide services that syntactically validate experimentally generated data files, covering commonly used and endorsed community data formats (e.g. FASTQ)
* Provide services that allow data contributors to confirm their submissions, prompting storage of data and metadata in the DCP data store
* Provide services for restricting access to only those submissions and their contents for which a user has either created themselves or been explicitly granted permission to view

### Supporting tools

* Provide tools that use reference (latest) metadata schemas, defined by the metadata team, to produce template spreadsheets in a canonical spreadsheet format, such that data contributors can easily complete this spreadsheet to capture their metadata
* Provide tools that allow data contributors to generate customised spreadsheets that target specific experiment types or data types based on a small number of screening criteria

### Dependencies

The Ingestion Service will be as loosely coupled as possible to schemas defined by the metadata team. The definition of a core domain model for data and metadata is therefore in scope for the Ingestion Service, as this core domain model must be shared between the Ingestion Service and Metadata Schema. This core domain model defines high level concepts (e.g. 'Biomaterial', 'Protocol', 'File'), and changes to this core model must be agreed between the responsible teams.

The Ingestion Service depends on the Upload Service for ingestion of data, and requires notifications of new file upload events in order to link those files to the appropriate metadata and to reference the cloud locations of those files.

The Ingestion Service has a dependency on the Data Store write APIs when storing data (writing bundles and files) upon completion of a submission. Additionally, the Ingestion Service has a dependency on the Data Store team to provide new bundle requirements, and the Metadata Schema team to provide the definition of bundle specifications, in order to support the generation of bundles from submitted data and metadata.

All services and tools that are used by direct data contributors will be rigorously tested with users to ensure a high quality user experience, and will take advantage of user experience experts within the DCP team.

### Scientific "guardrails"

The Ingestion Service requires oversight on which data types are required within the DCP (for example, SmartSeq2 and 10x sequencing data, imaging data) and by extension which file formats are endorsed and acceptable within the DCP (e.g. FASTQ, BAM, TIFF).

## Out-of-scope

### Interfaces

* Preview illustration of data in the user interface, for example as it "will appear" in the data portal
* Dedicated views over the data in the submission interface, for example visualisation of all data for a particular project or from a particular tissue type
* Searching over submitted data, beyond that required for retrieval of a given user's previous submissions

### Core capabilities

* Metadata specification definitions. Ingest will utilise metadata definitions defined by DCP-compliant JSON schemas, as defined by the metadata team, and will ensure the Ingest API is schema-agnostic where possible, but will not actively define metadata specifications beyond the restrictions placed by the shared core domain model outlined above
* Definition of bundle structures for the DCP datastore. Whilst the Ingest API will provide support for generating bundles in response to submissions, the Ingestion Service is not responsible for the specification of bundle structures - it is expected these will be provided either by the metadata team or to address specific consumer requirements
* Authentication services. The Ingestion Service will utilise DCP-wide authentication services. Authorisation services for specific ingest requirements are likely out of scope, unless DCP-wide authorisation services cannot fulfil requirements that are overly specific to ingest
* Automated conversion of "older" spreadsheets to newer versions
* Definition of additional error types, beyond those described by the ingest API validation services. The spreadsheet uploader will communicate any errors encountered during conversion of spreadsheet contents to JSON to the ingest API for review by the contributors (including, for example, via the ingest UI). Likewise, validation of spreadsheets is out of scope; instead, all contents of a spreadsheet (as long as it correctly asserts it's format e.g. .xlsx) must be converted to JSON.

### Supporting tools

* Generation of lab specific spreadsheets that are not directly generated from a JSON schema endorsed by the metadata team. Additional customisations (for example, modifying column names or adding additional tabs) for individual contributors will not be supported through any mechanism other than through the use of standard configuration (i.e. which modules are to be included) or through the proposal of metadata schema changes

## Milestones and Deliverables

  1. _**"Wrangler-assisted"** submission phase:_
     - Provision of services to allow data contributors to upload all required data and metadata. Process is heavily wrangler-led, with metadata exchanged manually (e.g. via spreadsheets emails to wranglers) and support provided to upload data. Contributors see very few DCP user interfaces
     - Delivery: **Q3 2018**
  1. _**"Online, wrangler assisted"** submission phase:_
     - Provision of services to allow wrangler-assisted submissions with user interfaces available to contributors to provide additional support, including views over all submissions for a single project, summaries of submissions in progress, and validation reports for submissions
     - Delivery: **Q1 2019**
  1. _**"Self-service"** submission phase:_
     - Delivery of services to transition from a wrangler-led submission process to a "self-service" submission process, including full-featured user interfaces to support spreadsheet based submissions
     - Delivery: **Q3 2019**
  1. _**"Self-service, review and repair"** submission phase:_
     - Delivery of services to provide support for the review and repair of potential errors or inconsistencies in submissions, including reviewing validation errors and providing updates for instant revalidation, reviewing ontology mappings, providing additional ontology mappings in a responsive user interface.
     - Delivery: **Q4 2019**
  1. _**"Dynamic evolution"** submission phase:_
     - Provision of ingest services that support data contributors in adding new fields to their submissions, which dynamically triggers metadata updates for the metadata team to review and accept during the submission process. Submitters are informed, via summary views in the user interface, when their data has been used in an analysis pipeline and results are available. Progress of submissions throughout the system - including references to external usage (e.g. via DCP data portals) can be viewed supporting transparent data attribution
     - Delivery: **2020**

## Roles

### Project Lead

[Tony Burdett](mailto:tburdett@ebi.ac.uk)

### Product Owner

[Norman Morrison](mailto:norman@ebi.ac.uk)

### Technical Lead

[Simon Jupp](mailto:jupp@ebi.ac.uk) (Interim, pending new hire)

## Communication

### Slack

[HumanCellAtlas #ingestion-service](https://humancellatlas.slack.com/messages/ingestion-service) for all discussions on the Ingestion Service

[HumanCellAtlas #dcp-reuse](https://humancellatlas.slack.com/messages/dcp-reuse) if you are interested in using the Ingestion Service, or other DCP components, in your own projects

### Mailing list

Team email: ingest-team@data.humancellatlas.org

## Github repositories

### Ingest Central

 * https://github.com/HumanCellAtlas/ingest-central
   * ingest-central is the hub repository for the Ingestion Service - use this to report issues or as the gateway to specific components

### Ingestion Service Deployment

 * https://github.com/HumanCellAtlas/ingest-kube-deployment
   * ingest-kube-deployment provides access to configurations and scripts needed to set up an entire Ingestion Service environment

### Ingestion Service Components

 * https://github.com/HumanCellAtlas/ingest-core
 * https://github.com/HumanCellAtlas/ingest-ui
 * https://github.com/HumanCellAtlas/ingest-broker
 * https://github.com/HumanCellAtlas/ingest-validator
 * https://github.com/HumanCellAtlas/ingest-archiver
 * https://github.com/HumanCellAtlas/ingest-auth
 * https://github.com/HumanCellAtlas/ingest-client
 * https://github.com/HumanCellAtlas/ingest-importer
 * https://github.com/HumanCellAtlas/ingest-backup
 * https://github.com/HumanCellAtlas/ingest-exporter
 * https://github.com/HumanCellAtlas/ingest-accessioner
 * https://github.com/HumanCellAtlas/ingest-state-tracking
 * https://github.com/HumanCellAtlas/ingest-fastq-validator
 * https://github.com/HumanCellAtlas/ingest-staging-manager
