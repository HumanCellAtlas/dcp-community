
# Metadata Schema

## Description

The Human Cell Atlas (HCA) Metadata Schema are JSON format schema. These schema are designed to capture and provide structure for the descriptive scientific metadata associated with HCA datasets. These schemas aim to ensure [the FAIRness of the HCA data](https://www.nature.com/articles/sdata201618).

These metadata schema aim to capture more than the minimum requirements for representing the samples and experiments conducted in the context of the Human Cell Atlas project but they do not aim to be truely comprehensive and capture every single variable that could be recorded about these samples and experiments. We are aiming for a middle ground that provides data consumers with rich enough information that they can use the data to answer biological questions but to not overburden the data contributors with an expectation to record every detail of every process they run in order to generate this data.

## Objectives

1. Ensuring that HCA data are findable and the schemas are computationally queryable
2. Ensuring that HCA metadata supports reproducibility and reusability
3. Ensuring that HCA metadata has a consistent representation and can accurately represent the scientific description of a dataset in a computational readable manner
4. Defining JSON schema which enable objectives 1 and 2
5. Defining the semantic rules that the content follows
6. Working with Ingest Infrastructure to specify the entity structure for HCA metadata schema
7. Defining style guides and the release process for the schema
8. Ensuring the defined metadata schema provides a positive user experience for those who read and write them

## In-scope

* Defining the specification for the schema and defining the schemas themselves 
* Defining the semantic rules for metadata content of the schema
* Incorporating community feedback to extend and improve the schema
* Ensuring the schema is compatible with open source software packages supporting the same draft of the JSON schema
* Providing a test framework to enable community members to test if their proposed extensions are functional and meet our style guides
* Providing exemplar data in the schema format
* Providing computationally readable information about depreciated and moved fields within the schema
* Working with data contributors to understand what types of descriptive fields they can supply for their samples and experiments
* Working with data consumers to understand what descriptive fields they need in order to be able to analyse and visualise the HCA data
* Extending the schema to meet the needs of data contributors and data consumers based on the feedback they provide

### Scientific "guardrails"

In order for the HCA metadata schema to meet the needs of the community, the defined schema must reflect the types of data which are being generated in order to build the Human Cell Atlas. We will seek guidance from the scientific leadership of the HCA to help understand and prioritize new data types for addition to the schema collection as well as collecting feedback from data contributors and data consumers.

## Out-of-scope

* Building interfaces to translate metadata between HCA metadata JSON and other serialization formats
* Building services to index or query the metadata

## Milestones and Deliverables

During 2018/2019 The metadata charter committee will work to complete the following milestones.

* Implement a test suite to enable independent testing of the metadata schemas outside of any other infrastructure
  - Delivery: **Q3 2018**
* Work with data contributors and consumers from the community who are conducting image based transcriptomics experiments to define metadata schema that can capture needed details of these experiments
  - Delivery: **Q1 2019**
* Work with data contributors and consumers from the community who are conducting single nuclei RNA-seq experiments to define metadata schema that can capture needed details of these experiments
  - Delivery: **Q1 2019**


## Roles

### Project Lead
[Laura Clarke](mailto:laura@ebi.ac.uk)
### Product Owner
[Norman Morrison](mailto:norman@ebi.ac.uk)
### Technical Lead
[Mark Diekhans](mailto:markd@ucsc.edu)

## Communication

### Slack

HumanCellAtlas/hca-metadata - for discussions about metadata needs and conventions

## Github repositories

[https://github.com/humancellatlas/metadata-schema](https://github.com/humancellatlas/metadata-schema)
