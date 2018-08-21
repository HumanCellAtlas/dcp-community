
# Expression Matrix Service

## Description

The expression matrix service enables researchers to easily access and analyze HCA expression matrix data.

## Objective

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
    * Compound metadata and numerical filtering (*combinations of the above filters*)

* Expression Service API and scalable backend that provides access to
query interface

* Architecture, software design, and scale testing for scalability

* Data representation
    * Expression matrix data format for transport
    * Expression matrix data format for storage
    * QC matrix data format for transport
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
    * ~~Decision on at-rest and transfer matrix data formats~~
    * Dependency on Query Service for join queries on metadata

## Out-of-scope

* Data visualization portal or web application for expression matrices
* Transformative data manipulation; analysis apps or pipelines using expression data (e.g. data normalization)
* Standards engagement for APIs or data storage formats
 with the GA4GH Large-Scale Genomics work stream

## Milestones

**EOY 2018** - Architecture complete; implementation of API feature complete, suitable for limited use, but not scalable

**Mid 2019** - Performant, highly scalable version, suitable for interactive (responsive) data serving across the entire HCA dataset

## Roles

### Project Lead

[Ambrose Carr](mailto:acarr@chanzuckerberg.com)

### Product Owner

[Brian Raymor](mailto:brianraymor@echanzuckerberg.com)

### Technical Lead

[Marcus Kinsella](mailto:mkinsella@chanzuckerberg.com)

## Communication

### Slack Channels

HumanCellAtlas/the-matrix-reloaded: Discussion of expression matrix service

## Github repositories

* https://github.com/HumanCellAtlas/matrix-service
* https://github.com/HumanCellAtlas/table-testing
