
# Expression Matrix Service

## Description

The Human Cell Atlas (HCA) Data Coordination Platform (DCP) ingests, processes, and stores single-cell data, making that data easily available to a wide audience of researchers. The single-cell data type of most interest to researchers is the expression matrix, the cell-by-gene expression values that are the starting point for many downstream analyses. To support this data type, the Expression Matrix Service defines a public API to return expression matrices in response to queries.

## Objectives

* Build, operate, and maintain a data access API that serves expression
matrices from the HCA DCP Data Store with options to subset data based on bundle metadata, genes, and numerical filters.

* Serve as a test bed for a re-usable metadata query service, created by
the *Query Service* component.

## In-scope

* Expression Matrix Service interface supports queries based on:
    * Bundle identifiers
        * This query enables selection of specific submissions.
    * Metadata (*across bundles; potentially provided directly by Query Module*)
        * These types of filters will enable users to select cells based on their biological characteristics, for example, all kidney cells, or all cells prepared using the 10x v2 assay.
    * Numerical filtering (*on gene expression values, and also QC metrics*)
        * These types of filters will, for example, enable users to query for cells according to their expression of key marker genes, or exclude cells with low quality scores.

* Expression Matrix Service API and scalable backend that provides access to
query interface

* Architecture, software design, and scale testing for scalability

* Data representation
    * Multiple expression matrix data formats based on _de facto_ standards delivered by the API and the Command Line Interface (CLI)
    * Expression matrix data format for storage
    * QC matrix data format delivered by the API and the Command Line Interface (CLI)
    * QC matrix data format for storage


* Extends HCA DCP Command Line Interface (CLI) client to support the Expression Matrix Service

* Documentation
    * Publish documentation for API and file formats
    * Publish example code for developers to enable fast/easy adoption of API by analysis and visualization portals

* User Experience
    * Conduct UX research and design; drive query and API design directly from use cases
    * Validate design with communities building portals for analysis and
    visualization
    * Provide feedback in the form of architecture sessions and written UX
    reports to the DCP community about the experience of building layered services using the public API(s). (e.g., *can the expression microservice be built using only public Data Store API?*)
    * Scientific "guardrails" to warn users when executed queries produce data subsets with questionable scientific utility
        * For example, if a user queries for the expression of only a single gene across all lung cells, this query would trigger a warning that the returned data cannot be effectively normalized

* External dependencies
    * Dependencies on Data Store, Query Service, and Secondary Analysis to enable and expose data matrices and quality control metrics
    * Dependency on Query Service for join queries on metadata

## Out-of-scope

* Compound metadata and numerical filtering (*combinations of the In-scope filters*)
* Data visualization portal or web application for expression matrices
* Transformative data manipulation; analysis apps or pipelines using expression data (e.g. data normalization)
* Standards engagement for APIs or data storage formats with the GA4GH Large Scale Genomics work stream prior to basic completion of matrix service implementation and integration of user feedback

## Milestones and Deliverables

**Nov 2018**: *Deploy a minimum viable product (MVP) feature set as part of HCA DCP Beta.*

Implementation of v0 API suitable for limited use but not scalable, includes:
* Queries based on bundle identifiers
* Delivery of highest priority expression matrix data formats based on de facto standards

**EOY 2018**: Implementation of v1 API, suitable for limited use but not scalable. Updating to include:
* Queries based on metadata filters
* Delivery of additional expression matrix data formats based on de facto standards

**Q1 2019**: Implementation of v2 API, suitable for limited use but not scalable. Updating to include:
* Queries based on numerical filters
* Feedback from HCA DCP Beta users

**Q1 2019**: DCP Command Line Interface (CLI) extended to support Expression Matrices

**Mid 2019**: API and Architecture feature complete and polished. *(Highly scalable and performant implementation suitable for responsive data serving across the entire HCA dataset)*

## Roles

### Project Lead

[Ambrose Carr](mailto:acarr@chanzuckerberg.com)

### Product Owner

[Brian Raymor](mailto:brianraymor@echanzuckerberg.com)

### Technical Lead

[Marcus Kinsella](mailto:mkinsella@chanzuckerberg.com)

## Communication

### Slack Channels

[HumanCellAtlas/the-matrix-reloaded](https://humancellatlas.slack.com/messages/the-matrix-reloaded): Discussion of expression matrix service

### Mailing List
Team email: expression-matrix-team@data.humancellatlas.org

## Github repositories

* https://github.com/HumanCellAtlas/matrix-service is the developer workspace for the Expression Matrix Service.
* https://github.com/HumanCellAtlas/table-testing is a collaborative space to specify requirements, examples, and tests for on-disk file formats used to store expression matrices arising from single-cell RNA sequencing analyses.
