
# Metadata Schema

## Description

The HCA Metadata schema committee creates and maintains the Human Cell Atlas metadata schema in JSON format. These schema are designed to capture and provide structure for the descriptive scientific metadata associated with HCA datasets. These schemas aim to ensure the FAIRness* of the HCA data [The FAIR Guiding Principles for scientific data management and stewardship](https://www.nature.com/articles/sdata201618). 

## Objective

1. Ensure that HCA data are findable and the schemas are computationally queryable.
2. Ensure that HCA metadata supports reproducibility and reusability.
3. Ensure that HCA metadata has a consistent representation and is computationally queryable.
4. Define JSON schema which enable objectives 1 and 2.
5. Specify the entity structure for HCA metadata schema.
6. Define style guides and release process.
7. Ensure the defined metadata schema provides a positive user experience for those who read and write them.

## In-scope

* Defining the specification for the schema and the schema itself.
* Incorporate community feedback to extend and improve the schema.
* Ensuring the schema is compatible with open source software packages supporting the same draft of the JSON schema.
* Provide a test framework to enable community members to test if their proposed extensions are functional and meet our style guides.
* Providing exemplar data in the schema format.
* Working with data contributors to understand what types of descriptive fields they can supply for their samples and experiments.
* Working with data consumers to understand what descriptive fields they need in order to be able to analyse and visualise the HCA data.
* Extending the schema to meet the needs of data contributors and data consumers based on the feedback they provide.

### Scientific "guardrails" 

In order for the HCA metadata schema to meet the needs of the community, the defined schema must reflect the types of data which are being generated in order to build the Human Cell Atlas. The HCA Metadata schema committee will seek guidance from the scientific leadership of the HCA to help understand and prioritize new data types for addition to the schema collection as well as collecting feedback from data contributors and data consumers.

## Out-of-scope

* Building interfaces to translate metadata between HCA metadata JSON and other serialization formats
* Building services to index or query the metadata

## Milestones

During 2018/2019 The metadata charter committee will work to:

* Implement a test suite to enable independent testing of the metadata schemas outside of any other infrastructure
* Work with data contributors and consumers from the community who are conducting image based transcriptomics experiments to define metadata schema that can capture needed details of these experiments
* Work with data contribytors and consumers from the community who are conducting single nucleai RNA-seq experiments to define metadata schema that can capture needed details of these experiments

## Roles

### Project Lead
[Laura Clarke](mailto:laura@ebi.ac.uk)
### Product Owner
[Norman Morrison](mailto:norman@ebi.ac.uk)
### Technical Lead


## Communication

### Slack

HumanCellAtlas/hca-metadata - for discussions about metadata needs and conventions

## Github repositories

[https://github.com/humancellatlas/metadata-schema](https://github.com/humancellatlas/metadata-schema)
