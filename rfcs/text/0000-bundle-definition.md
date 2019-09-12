### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# HCA DCP Bundle Types and Definitions

## Summary

There are currently no documented definitions for what a "bundle" is in the HCA DCP, including no definitions of what they are, what they contain, or who they support. Lack of clarity causes confusion for data consumers about how the data and metadata in the DCP are structured and organized. Here we propose to formalize a bundle definition, bundle type definitions, target users and consumers, and specific use cases in order to support the needs of data consumers.

Specifically, the following charters will be supported by this RFC:
- the Data Store (DSS) [charter](https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/DataStore/charter.md#core-capabilities) ("The Data Store owns the bundle use cases and bundle type definitions, while the precise specifications will be negotiated between the Metadata and other teams.");
- the Ingestion Service [charter](https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/IngestionService/charter.md#dependencies) ("The Ingestion Service has a dependency on the Data Store team to provide new bundle requirements, and the Metadata Schema team to provide the definition of bundle specifications, in order to support the generation of bundles from submitted data and metadata."); and
- the Metadata Schema [charter](https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/MetadataSchema/charter.md#objectives) ("Working with the Data Store to receive new bundle structure requirements and defining a new specification to be implemented by the Ingestion Service team."). 


## Author(s)

*Recommended format for Authors:*

 [Mallory Freeberg](mailto:mfreeberg@ebi.ac.uk)
 
 [Brian Hannafious](mailto:bhannafi@ucsc.edu)
 
 [Hannes Schmidt](mailto:hannes@ucsc.edu)
 
 [Andrey Kislyuk](mailto:akislyuk@chanzuckerberg.com)

> Replace Mallory with Zina P and/or Chris V.

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

*Describe the user or technical need in detail [with alignment to the DCP roadmap priorities where possible]. Link prior community discussions to demonstrate support for this RFC.*

There are currently no documented definitions for what a "bundle" is in the HCA DCP, including no definitions of what they are, what they contain, or who they support. Lack of clarity causes confusion for data consumers (and DCP developers) about how the data and metadata in the DCP are structured and organized. At the June 2019 DCP F2F meeting in Cambridge, a breakout session was held to discuss "What is a bundle?", and a lot of useful discussion occurred. This RFC will formally document what a bundle is, anchoring the definition on how bundles support HCA DCP data consumers. This RFC will also describe new bundles types that could be supported by the HCA DCP.

### User Stories

*Share the [User Stories](https://www.mountaingoatsoftware.com/agile/user-stories) motivating this RFC.*

1. As a data consumer who develops data processing tools (*e.g.* a sequence read aligner), I would like to browse, query, and get primary data from the DSS so that I can use it as testing data during development.
1. As a data consumer who develops analysis tools (*e.g.* a clustering algorithm), I would like to browse, query, and get alignment data from the DSS so that I can use it as testing data during development.
1. As a data consumer who is a computational biologist, I would like to get expression matrices from the DSS so that I can compare against data I already have.
1. As a data consumer who is a biologist, I would like to get expression matrices from the DSS so that I can see the expression profile of &lt;my gene of interest&gt; in &lt;my cell type(s) or organ(s) of interest&gt;.
1. As a data consumer who is a computational biologist, I would like to know what reference data was used to generate processed data in the DSS and I would like to be able to get it for myself so that I can process/analyze my own data with the same references as HCA DCP data.


## Scientific "guardrails" [optional]

*Describe recommended or mandated review from HCA Science governance to ensure that the RFC addresses the needs of the scientific community.*

## Detailed Design

*Explain the design in sufficient detail such that the implementation and (if appropriate) the interaction with existing DCP software are both reasonably clear.*

### General bundle definition

In the HCA DCP, a bundle is defined as: 
1. a transaction of files that belong together for scientific reasons - **OR** - 
1. an association between a set of files (*e.g.* data, metadata)

> These two definitions came out of the F2F bundle session and were generally agreed to make sense by different groups. Should we go with just one of these? Do we merge them together? Do we accept both? Should there be a vote?

### Bundle types and their definitions

Bundle types, definitions, consumers, and specifications are outlined below. Use cases from above are indicated where supported by a specific bundle type.

#### Table header definitions
- **Type**: The name of the bundle type
- **Definition**: What is and is not in this bundle type
- **Target consumer**: Who will be consuming this bundle type
- **Use cases**: Which Users Stories are supported by this bundle type

| Type | Definition | Target consumer | Use cases |
|:-|:-|:-|:-|
| Primary bundle | A bundle that contains primary (raw) data files and all related metadata files. | computational biologists; Data Processing Pipelines; Data Browser; Query Service | 1 |
| Secondary bundle | A bundle that contains secondary (alignment, expression) data files produced by the Data Processing Pipelines, input primary data files, and all related metadata files. | computational biologists; Data Browser; Query Service; Matrix Service | 2, 3, 4(?) |
| Tertiary bundle* | A bundle that contains tertiary (expression) data files produced by the Matrix Service, input primary and secondary data files, and all related metadata files. | computational biologists; biologists; Data Browser; Query Service | 3, 4 |
| Project bundle* | A bundle that contains project-level metadata files. This bundle type does not contain data files or metadata files related to biomaterials, protocols, processes, or files. | computational biologists; biologists; | ? |
| Resource/Reference bundle* | A bundle that contains reference data files used by the Data Processing Pipelines(?) and all related metadata files. This bundle type does not contain data files or metadata files related to biomaterials, protocols, processes(, or projects?). | computational biologists; Data Processing Pipelines; | 2, 3, 5 |
| DAPS bundle* | Definition discussion ongoing in [PR #88](https://github.com/HumanCellAtlas/dcp-community/pull/88). | ? | ? |

> Asterisk (*) indicates bundles types that do not currently exist in the DCP but have been discussed as potential future bundle types to support data consumers.

### New bundle types and their definitions

> **NB**: Describing and defining new bundle types probably belong in their own design RFC, but they are included here as they were a natural extension of defining the current two bundle types. Consider moving the "New bundle types and their definitions" section to a new design RFC.

The only bundle types that currently exist in the DSS are Primary and Secondary bundles. The remaining bundle types are proposed based on data consumer needs and are described in more detail below.

#### New Tertiary bundle

Describe here...

- Produced by Matrix Service. *Will anyone else be producing expression data files using the output of Data Processing Pipelines as input?*
- In the first instance will contain expression data files representing one project. In the future, will contain expression data files representing partials projects or multiple projects.
- 

#### New Project bundle

Describe here...

- Will contain project-level metadata only. 
- Unclear how this bundle will be used by data consumers.

#### New Resource/Reference bundle

Describe here...

- Should we call it "Resource" or "Reference" bundle?
- Who will generate/maintain these? Data Processing Pipelines?

### Acceptance Criteria [optional]

*Acceptance criteria are the conditions that a RFC must satisfy to be accepted by users or other stakeholders.* 

1. DCP developers have a shared understanding of what a bundle is, including what is in a bundle and how bundles relate to one another.

### Unresolved Questions

*What aspects of the design do you expect to clarify further through the RFC review process?*
1. What is the process for adding or adopting new bundle types? Changing definition of current bundle types?
1. What other bundle specifications should be included here, especially in light of the bundle type design RFC ([PR link](https://github.com/HumanCellAtlas/dcp-community/pull/86))?

*What aspects of the design do you expect to clarify later during iterative development of this RFC?*
1. Does this informational RFC depend on mechanics of copy-forward?
1. What bundle types should be defined in this RFC? What types should be defined in future RFCs?


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
