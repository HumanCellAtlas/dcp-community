### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# HCA DCP Bundle Types and Definitions

## Summary

There are currently no documented definitions for what a "bundle" is in the HCA DCP, including no definitions of what
they are, what they contain, or who they support. Lack of clarity causes confusion for data consumers about how the data
and metadata in the DCP are structured and organized. This RFC formalizes *de facto* data bundle types, definitions,
target users/consumers, and specific use cases to improve clarity and confidence among DCP developers when working with
the DCP data model.

Specifically, the following charters will be supported by this RFC:

- The Data Store (DSS)
  [charter](https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/DataStore/charter.md#core-capabilities)
  ("The Data Store owns the bundle use cases and bundle type definitions, while the precise specifications will be
  negotiated between the Metadata and other teams.");

- The Ingestion Service
  [charter](https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/IngestionService/charter.md#dependencies)
  ("The Ingestion Service has a dependency on the Data Store team to provide new bundle requirements, and the Metadata
  Schema team to provide the definition of bundle specifications, in order to support the generation of bundles from
  submitted data and metadata."); and

- The Metadata Schema
  [charter](https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/MetadataSchema/charter.md#objectives)
  ("Working with the Data Store to receive new bundle structure requirements and defining a new specification to be
  implemented by the Ingestion Service team.").

## Author(s)

* [Mallory Freeberg](mailto:mfreeberg@ebi.ac.uk) (TODO: Replace Mallory with Zina P and/or Chris V.)
 
* [Brian Hannafious](mailto:bhannafi@ucsc.edu)
 
* [Hannes Schmidt](mailto:hannes@ucsc.edu)
 
* [Andrey Kislyuk](mailto:akislyuk@chanzuckerberg.com)

## Shepherd

* [Andrey Kislyuk](mailto:akislyuk@chanzuckerberg.com)

## Motivation

There are currently no documented definitions for what a "bundle" is in the HCA DCP, including no definitions of what
they are, what they contain, or who they support. Lack of clarity causes confusion for data consumers (and DCP
developers) about how the data and metadata in the DCP are structured and organized. At the June 2019 DCP F2F meeting in
Cambridge, a breakout session was held to discuss "What is a bundle?", and a lot of useful discussion occurred. This RFC
will formally document what a bundle is, anchoring the definition on how bundles support HCA DCP data consumers. This
RFC will also describe new bundle types that could be supported by the HCA DCP.

### User Stories

1. As a data consumer who develops data processing tools (*e.g.* a sequence read aligner), I would like to browse,
   query, and get primary data from the DSS so that I can use it as testing data during development.

1. As a data consumer who develops analysis tools (*e.g.* a clustering algorithm), I would like to browse, query, and
   get alignment data from the DSS so that I can use it as testing data during development.

1. As a data consumer who is a computational biologist, I would like to get expression matrices from the DSS so that I
   can compare against data I already have.

1. As a data consumer who is a biologist, I would like to get expression matrices from the DSS so that I can see the
   expression profile of &lt;my gene of interest&gt; in &lt;my cell type(s) or organ(s) of interest&gt;.

1. As a data consumer who is a computational biologist, I would like to know what reference data was used to generate
   processed data in the DSS and I would like to be able to get it for myself so that I can process/analyze my own data
   with the same references as HCA DCP data.

1. As a tool developer, I would like to identify and get raw data in the DSS so that I can run my data processing and
   analysis tools.

1. As a tool developer, I would like to identify and get alignment results and count matrices in order to produce
   expression matrices.

1. As an external user of the DSS who is developing analysis tools, I would like to browse, query, and get raw and
   processed data based on some rough criteria so that I can understand what the data are and develop my tool.

1. As an Azul or Query Service developer I want to be able to distinguish between different types of bundles in order to
   know which bundle to index, link, etc. or to provide bundle types to users as a search criterion.

1. As a computational biologist, I would like to use the same references that the HCA DCP uses in their analysis.

## Detailed Design

### What is a bundle?

A bundle is a unit of data organization, roughly equivalent to a folder or directory. In the HCA DCP, a bundle is
defined as either a transactional grouping of files that belong together for scientific reasons, or a useful association
between a set of files for utility purposes.

* Data bundles contain the results of a single data transaction. They can be viewed as the digital equivalent of an
  "instrument run" - a quantity of data gathered from a single process instance. Data bundles are annotated with
  metadata using a procedure designed by the metadata team.

* Utility bundles exist to organize (group) data bundles and represent data of non-DCP origin (such as reference data)
  that is used in the course of analysis.

### How are bundle types defined?

Operationally, bundle types can be defined like file types or media types: they are used as a data type identifier for
applications to pattern match on whether data should be considered as suitable for input to the application.

### Bundle types and their definitions

Bundle types, definitions, consumers, and specifications are outlined below. Use cases from above are indicated where
supported by a specific bundle type.

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

> Asterisk (*) indicates bundle types that do not currently exist in the DCP but have been discussed as potential future
> bundle types to support data consumers.

### New bundle types and their definitions

The only bundle types that currently exist in the DSS are Primary and Secondary bundles. The remaining bundle types are
proposed based on data consumer needs and are described in more detail below.

#### New Tertiary bundle

- Produced by Matrix Service. *Will anyone else be producing expression data files using the output of Data Processing
  Pipelines as input?*

- In the first instance will contain expression data files representing one project. In the future, will contain
  expression data files representing partials projects or multiple projects.

#### New Project bundle

- Will contain project-level metadata only. 

- Can be used to group data for presentation to data consumers.

#### New Resource/Reference bundle

- Should we call it "Resource" or "Reference" bundle?

- Who will generate/maintain these? Data Processing Pipelines?

### When is a new bundle type needed?

* There are 3 classes of data bundles currently provided or concretely envisioned in DCP: primary data bundles,
  secondary analysis bundles, and tertiary (meta-analysis or aggregate analysis) bundles. A new class of bundle could be
  needed if a new analysis modality or data aggregation method were to be introduced.

* As denoted in the bundle type notation sketch below, [RFC 7231](https://tools.ietf.org/html/rfc7231) parameters can be
  used to designate the process that generated a bundle. A new class of process (for example, a new secondary analysis
  pipeline) could cause a new `bundle-type` value with the same bundle type and subtype as previous secondary analysis
  data bundles, but with a new parameter value for `hca-pipeline`.

* Similarly, a new modality or format of primary experimental output data would cause a new primary data bundle
  `bundle-type` value.

### Conveying bundle types

A new field, `bundle-type`, will be introduced to the DSS bundle manifest (the response to `GET bundle` in DSS). The
field value is a string formatted in a similar manner
to [DCP Media Types](https://docs.google.com/document/d/1TqihrgXjct9aDmTJO52_gE2WlpFysB1OkG9C8exmWTw) and consistent
with the syntax defined in [RFC 7231](https://tools.ietf.org/html/rfc7231#section-3.1.1.1). DSS must require the bundle
type to be specified when creating a new bundle, but should not enforce the vocabulary. DSS must allow subscriptions
and search results to be predicated on bundle type using the same string predicates available with other metadata and
manifest string fields.

### Registry of Bundle Types

Introduce a registry of bundle types to be maintained by the Data Store team with specification input from the Metadata
team. The initial contents of the registry are:

| Bundle Type              | Bundle Class        | Example `bundle-type` value                                 |
|--------------------------|---------------------|-------------------------------------------------------------|
| Project Metadata Bundle  | Utility bundle      | `hca/project`                                               |
| Primary Sequence Bundle  | Primary data bundle | `hca/primary-data; hca-data-type:SmartSeq2`                 |
| Primary Imaging Bundle   | Primary data bundle | `hca/primary-data; hca-data-type:sptx`                      |
| Secondary Analysis Bundle| Analysis data bundle| `hca/analysis-output; hca-pipeline:snap-atac`               |
| Reference Resource Bundle| Utility bundle      | `hca/analysis-reference; hca-resource-type:reference-genome`|
| Expression Matrix Bundle | Analysis data bundle| `hca/analysis-output; hca-resource-type:expression-matrix`  |

The registry would be maintained either as part of this git repository (HumanCellAtlas/dcp-community) or as part of a
repository chosen by the Data Store team, subject to a pull request-based contribution model.

### Acceptance Criteria [optional]

*Acceptance criteria are the conditions that a RFC must satisfy to be accepted by users or other stakeholders.* 

1. DCP developers have a shared understanding of what a bundle is, including what is in a bundle and how bundles relate
   to one another.

### Unresolved Questions

*What aspects of the design do you expect to clarify further through the RFC review process?*

1. What is the process for adding or adopting new bundle types? Changing definition of current bundle types?

1. What other bundle specifications should be included here, especially in light of the bundle type design RFC
   ([PR link](https://github.com/HumanCellAtlas/dcp-community/pull/86))?

*What aspects of the design do you expect to clarify later during iterative development of this RFC?*

1. Does this informational RFC depend on mechanics of copy-forward?

1. What bundle types should be defined in this RFC? What types should be defined in future RFCs?

1. What defines a "correct" bundle type?

1. Is the level of detail for the operational requirements of a bundle registry sufficient?

### Drawbacks and Limitations [optional]

*Why should this RFC **not** be implemented?*

### Prior Art [optional]

*Share references to prior art to deepen community understanding of the RFC, such as learnings, adaptations from earlier designs, or community standards.*

- [Submission to bundles SOP (Apr 2018)](https://docs.google.com/document/d/1x8mYLU8ubpZtTrzkwJrft1heqReX-pJLjJfb2X0fd2w)

- [Bundle metadata schema update ticket](https://github.com/HumanCellAtlas/metadata-schema/issues/986)

### Alternatives [optional]

*Highlight other possible approaches to delivering the value proposed in this RFC. 
What other designs were explored? What were their advantages? What was the rationale for rejecting alternatives?*

- Status Quo: no well-defined bundle types. Existing DCP components continue to infer bundle types via ad hoc methods
  and included metadata. No one *really* knows what a bundle is, or there are many different ideas about what a bundle
  is.
