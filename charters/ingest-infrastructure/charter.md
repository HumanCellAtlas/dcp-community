
# Ingest Infrastructure

## Description

The DCP Ingest Infrastructure provides a suite of services designed to facilitate data contributors providing data and metadata to the Human Cell Atlas data coordination platform. Ingest Infrastructure supports both *direct data contributors* (i.e. data generators working in labs that wish to provide data to the DCP via a "self-service" mechanism) and *data wranglers* (i.e. dedicated HCA staff working in conjunction with labs to provide assistance with the collection of data and metadata). Services offered by Ingest Infrastructure include:
  * Upload of metadata describing biological materials, data files, and the experimental processes that generated them
  * Validation of metadata against community-endorsed standards
  * Interaction with the DCP Upload Service to track and validate uploaded data files
  * Tracking of submission states, describing the life-cycle of experimental submissions through the DCP
  * Generation of identifiers for data, metadata and submissions
  * Storage of submitted data to the DCP Data Store

## Objective

Ingest Infrastructure supports data contributors in the collection, validation and submission of data and metadata at the point of generation, following endorsed community standards to ensure experimental data and analysis results are findable, accessible, interoperable and reusable for downstream consumers.

The primary mission of ingest infrastructure is to define a series of application programming interfaces (**'Ingest APIs'**) for the submission of data and metadata that adheres to community standards, as defined by the DCP metadata team, to be stored in the DCP datastore.

The secondary mission is to work with data contributors and data wranglers to provide more convenient methods of submission (over the more low-level programmatic access), by developing a single canonical ingest user interface (**'Ingest UI'**) and tools to support submission of spreadsheet-based metadata, including the generation of templates that capture required metadata (**'Spreadsheet template generator'**) and clients that can submit spreadsheets in this format to ingest APIs (**'Spreadsheet uploader'**).

Ingest Infrastructure aims to collect experimental metadata at a level of granularity that make sense to data contributors, and which may not necessarily be the same as that required by a data consumer. This level of granularity will be referred to as a *'submission'* in this charter. Submissions do not correspond to the unit of storage in the data store, or the unit of data expected by the analysis pipelines.

## In-scope

### Ingest API
 * Develop and maintain a RESTful API supporting the collection of experimental metadata via submissions
 * Provide validation services that supply detailed reports, available through this API, on the quality of submitted data with respect to the community-endorsed schemas delivered by the metadata team
 * Provide state tracking services that supply information about the current state of any given submission, allowing submitters to see where in the ingestion process their data is currently
 * Provide services for connecting metadata describing experimentally generated data files, and the locations of those data files in the cloud
 * Provide services that syntactically validate experimentally generated data files, covering commonly used and community endorsed data formats (e.g. FASTQ).
 * Provide services that allow data contributors to confirm submissions, prompting data and metadata to be stored in the DCP data store for downstream access (i.e. the generation of datastore *bundles*)
 * Provide services for restricting access to only those submissions, and their contents, for which a user has either created themselves or been explicitly granted permission to view


### Ingest UI
 * Develop and maintain a single 'endorsed' user interface that can be used either by *direct data contributors* or *data wranglers* to create, edit and validate submissions
 * Provide user interfaces allowing a data contributor to track the state of their ongoing submissions
 * Provide user interfaces allowing a data contributor to see all submissions they have made to the DCP
 * Provide user interfaces allowing data contributors to see validation reports about the contents of their submissions (including data file validation reports)
 * Provide user interfaces that allow data contributors to review and repair any metadata that fails to meet validation criteria, or has been automatically modified (for example, the automatic annotation of data with ontology terms)
 * Provision of services that allow ingest API clients to communicate errors they have encountered during data brokering. It is expected that clients will communicate enough information to allow a contributor to review what went wrong with a submission through the ingest UI, and if possible to take action to repair the erroneous contents


### Spreadsheet Uploader
* Develop and maintain services that submits metadata, supplied in the form of a canonical spreadsheet format to ingest APIs
* Ensure metadata in spreadsheets can be captured comprehensively (i.e. capture metadata that fully describes all aspects of an experiment that may be required for downstream analysis), even if this **does not currently validate** against endorsed metata schemas
* Provide services to allow spreadsheets to be *re-uploaded* in order to make updates to existing submissions. This can place additional restrictions on the spreadsheets (for example, requiring the addition of a UUID field in order to identify the target records to update)


### Spreadsheet Template generator
 * Provision of tools that use reference (latest) metadata schemas, defined by the metadata team, to produce template spreadsheets in a canonical spreadsheet format, such that data contributors can easily fill in their metadata
 * Provision of services that allow data contributors (normally, data wranglers) to generate customised spreadsheets that target specific experiment types or data types based on a small number of screening criteria


To accomplish Ingest Infrastructure objectives, ingest infrastructure will be as loosely coupled as possible to schemas defined by the metadata team. As such, the definition of a *core domain model* is in scope for ingest infrastructure, as this core domain model for data and metadata must be shared between ingest infrastructure and metadata schema. This core domain model defines high level concepts (e.g. *'Biomaterial'*, *'Protocol'*, *'File'*), and changes to this shared kernel must be agreed between the responsible teams. All services and tools that are intended to be used by direct data contributors will be rigorously tested with users to ensure a high quality user experience, and will take advantage of user experience experts within the DCP team.


### Scientific "guardrails"

Ingest Infrastructure requires oversight on which data types are required within the DCP (for example, SmartSeq2 and 10x sequencing data, imaging data) and by extension which file formats are endorsed and acceptable within the DCP (e.g. FASTQ, BAM, TIFF).


## Out-of-scope

### Ingest API
 * Metadata specification definitions. Ingest will utilise metadata definitions defined by DCP-compliant JSON schemas, as defined by the metadata working group, and will ensure the Ingest API is schema-agnostic where possible, but will not actively define metadata specifications beyond the restrictions placed by the shared core domain model outlined above
 * Definition of bundle structures for the DCP datastore. Whilst the Ingest API will provide support for generating bundles in response to submissions, ingest infrastructure is not responsible for the specification of bundle structures - it is expected these will be provided either by the metadata team or to address specific consumer requirements
 * Authentication services. Ingest infrastructure will utilise DCP-wide authentication services. Authorisation services for specific ingest requirements are likely out of scope, unless DCP-wide authorisation services cannot fulfil requirements that are overly specific to ingest


### Ingest UI
 * Preview illustration of data, for example as it "will appear" in the data portal
 * Searching over submitted data, beyond that required for retrieval of a given user's previous submissions
 * Dedicated views over the data, for example visualisation of all data for a particular project or from a particular tissue type


### Spreadsheet uploader
 * Automated conversion of "older" spreadsheets to newer versions. Code developed in the spreadsheet uploader may be repurposed and provided as a convenience tool for wranglers, but is out of scope as part of ingest infrastructure
 * Definition of additional error types, beyond those described by the ingest API validation services. The spreadsheet uploader will communicate any errors encountered during conversion of spreadsheet contents to JSON to the ingest API for review by the contributors (including, for example, via the ingest UI). Likewise, validation of spreadsheets is out of scope; instead, all contents of a spreadsheet (as long as it correctly asserts it's format e.g. .xlsx) must be converted to JSON.


### Spreadsheet Template generator
 * Generation of lab specific spreadsheets that are not directly generated from a JSON schema endorsed by the metadata team. Additional customisations (for example, modifying column names or adding additional tabs) for individual contributors will not be supported through any mechanism other than through the use of standard configuration (i.e. which modules are to be included) or through the proposal of metadata schema changes


## Milestones

  1. _**"Wrangler-assisted"** submission phase:_
     - Provision of services to allow data contributors to deposit all required data and metadata. Process is heavily wrangler-led, with metadata exchanged manually (e.g. via spreadsheets emails to wranglers) and support provided to upload data. Contributors see very few DCP user interfaces
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

[HumanCellAtlas #ingestion-service](https://humancellatlas.slack.com/messages/ingestion-service) for all discussions on the DCP ingestion services

[HumanCellAtlas #dcp-reuse](https://humancellatlas.slack.com/messages/dcp-reuse) if you are interested in using ingest, or other DCP components, in your own projects


## Github repositories

### Ingest Central

 * https://github.com/HumanCellAtlas/ingest-central
   * ingest-central is the hub repository for DCP ingestion services - use this to report issues or as the gateway to specific ingest infrastructure componentents and microservices


### Ingest Infrastructure Deployment

 * https://github.com/HumanCellAtlas/ingest-kube-deployment
   * ingest-kube-deployment provides access to configurations and scripts needed to set up an entire ingest infrastructure environment (including `make` files for terraform and kubernetes configurations)

### Ingest Infrastructure Microservices componentents

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
