### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# HCA DCP Bundle Types and Definitions

## Summary

There are currently no documented definitions for what a "bundle" is in the HCA DCP, including no definitions of what they are, what they contain, or who they support. Lack of clarity causes confusion for data consumers about how the data and metadata in the DCP are structured and organized. Here we propose to formalize bundle types, definitions, target users and consumers, and specific use cases in order to support the needs of data consumers.

Specifically, the following charters will be supported:
- the Data Store (DSS) [charter](https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/DataStore/charter.md#core-capabilities) ("The Data Store owns the bundle use cases and bundle type definitions, while the precise specifications will be negotiated between the Metadata and other teams.");
- the Ingestion Service [charter](https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/IngestionService/charter.md#dependencies) ("The Ingestion Service has a dependency on the Data Store team to provide new bundle requirements, and the Metadata Schema team to provide the definition of bundle specifications, in order to support the generation of bundles from submitted data and metadata."); and
- the Metadata Schema [charter](https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/MetadataSchema/charter.md#objectives) ("Working with the Data Store to receive new bundle structure requirements and defining a new specification to be implemented by the Ingestion Service team."). 


## Author(s)

*Recommended format for Authors:*

 `[Mallory Freeberg](mailto:mfreeberg@ebi.ac.uk)`
 
 `[Brian Hannafious](mailto:bhannafi@ucsc.edu)`
 
 `[Hannes Schmidt](mailto:hannes@ucsc.edu)`
 
 `[Andrey Kislyuk](mailto:akislyuk@chanzuckerberg.com)`
 
 `[Query service person](mailto:)`

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

*Describe the user or technical need in detail [with alignment to the DCP roadmap priorities where possible]. Link prior community discussions to demonstrate support for this RFC.*

There are currently no documented definitions for what a "bundle" is in the HCA DCP, including no definitions of what they are, what they contain, or who they support. Lack of clarity causes confusion for DCP developers and data consumers about how the data and metadata in the DCP are structured and organized. At the June 2019 DCP F2F meeting in Cambridge, a breakout session was held to discuss "What is a bundle?", and a lot of useful discussion occurred. **MORE**

### User Stories

*Share the [User Stories](https://www.mountaingoatsoftware.com/agile/user-stories) motivating this RFC.*

1. As a data consumer who develops data processing tools (*e.g.* a sequence read aligner), I would like to browse, query, and get primary data from the DSS so that I can use it as testing data during development.
1. As a data consumer who develops analysis tools (*e.g.* a clustering algorithm), I would like to browse, query, and get alignment data from the DSS so that I can use it as testing data during development.
1. As a data consumer who is a computational biologist, I would like to get expression matrices from the DSS so that I can compare against data I already have.
1. As a data consumer who is a biologist, I would like to get expression matrices from the DSS so that I can see the expression profile of <my gene of interest> in <cell type(s) or organ(s) of interest>.
1. As a data consumer who is a computational biologist, I would like to know what reference data was used to generate processed data in the DSS and I would like to be able to get it for myself so that I can compare with my own research.


## Scientific "guardrails" [optional]

*Describe recommended or mandated review from HCA Science governance to ensure that the RFC addresses the needs of the scientific community.*

## Detailed Design

*Explain the design in sufficient detail such that the implementation and (if appropriate) the interaction with existing DCP software are both reasonably clear.*

### General bundle definition

In the HCA DCP, a bundle is defined as: 
1. a transaction of files that belong together for scientific reasons - **OR** - 
1. an association between a set of files (*e.g.* data, metadata)

### Bundle types and their definitions

Bundle types, definitions, consumers, and specifications are outlined below. Use cases from above are indicated where supported by a specific bundle type.

#### Table header definitions
- **Type**: The name of the bundle type
- **Definition**: What is and is not in this bundle type
- **Target consumer**: Who will be consuming this bundle type
- **Use cases**: Which Users Stories are supported by this bundle type
- **Specification**: Technical details about the bundle type (**MAY BE OUT OF SCOPE FOR THIS RFC**)

| Type | Definition | Target consumer | Use cases | Specification |
|:-|:-|:-|:-|:-|
| Primary bundle | A bundle that contains primary (raw) data files and all related metadata files. | computational biologists; Data Processing Pipelines; Data Browser; Query Service | 1 | Sequencing: type = sequence; Imaging: type = image |
| Secondary bundle | A bundle that contains secondary (alignment, expression) data files produced by the Data Processing Pipelines, input primary data files, and all related metadata files. | computational biologists; Data Browser; Query Service; Matrix Service | 2, 3 | Sequencing: type = [alignment, expression]; Imaging: type = [expression, ?] |
| Tertiary bundle | A bundle that contains tertiary (expression) data files produced by the Matrix Service, input primary and secondary data files, and all related metadata files. | computational biologists; biologists; Data Browser; Query Service | 2, 3 | type = [expression, ?] |
| Project bundle | A bundle that contains project-level metadata files. This bundle does not contain data files. | computational biologists; biologists; | 1, 2, 3, 4 | ? |
| Resource/Reference bundle | A bundle that contains reference data files used by the Data Processing Pipelines(?) and all related metadata files. | computational biologists; Data Processing Pipelines; | 4 | ? |

The only bundle types that currently exist in the DSS are Primary and Secondary bundles. The remaining bundle types are proposed based on user needs and are described in more detail below.

#### New Tertiary bundle

Describe here...

#### New Project bundle

Describe here...

#### New Resource/Reference bundle

Should we call it "Resource" or "Reference"? Describe here...

### Acceptance Criteria [optional]

*Acceptance criteria are the conditions that a RFC must satisfy to be accepted by users or other stakeholders.* 

1. Data consumers can make use of data from the DCP without knowing what a bundle is.
1. DCP developers have a shared understanding of what a bundle is, including what is in a bundle and how bundles relate to one another.

### Unresolved Questions

*What aspects of the design do you expect to clarify further through the RFC review process?*
1. What is the process for adding or adopting new bundle types? Changing current bundle types?
1. What other bundle specifications should be included here, especially in light of the bundle type design RFC?

*What aspects of the design do you expect to clarify later during iterative development of this RFC?*
1. Does this informational RFC depend on mechanics of copy-forward?


### Drawbacks and Limitations [optional]

*Why should this RFC **not** be implemented?*

### Prior Art [optional]

*Share references to prior art to deepen community understanding of the RFC, such as learnings, adaptations from earlier designs, or community standards.*

- [Submission to bundles SOP (Apr 2018)](https://docs.google.com/document/d/1x8mYLU8ubpZtTrzkwJrft1heqReX-pJLjJfb2X0fd2w/edit#heading=h.yciwsskdtgsq)
- [Bundle metadata schema update ticket](https://github.com/HumanCellAtlas/metadata-schema/issues/986)

### Alternatives [optional]

*Highlight other possible approaches to delivering the value proposed in this RFC. 
What other designs were explored? What were their advantages? What was the rationale for rejecting alternatives?*

Status quo: No one *really* knows what a bundle is or there are many different ideas about what a bundle is. Chaos ensues.
