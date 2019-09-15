### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# RFC: Bundle Types

## Summary

Formalizes data bundle types, definitions, target users/consumers, and specific use cases to improve clarity and
confidence among DCP developers when working with the DCP data model.

## Author(s)

* [Mallory Freeberg](mailto:mfreeberg@ebi.ac.uk)
* [Brian Hannafious](mailto:bhannafi@ucsc.edu)
* [Hannes Schmidt](mailto:hannes@ucsc.edu)
* [Andrey Kislyuk](mailto:akislyuk@chanzuckerberg.com)

## Shepherd

* [Andrey Kislyuk](mailto:akislyuk@chanzuckerberg.com)

## Motivation

There are currently no documented definitions for what a "bundle" is in the HCA DCP, including no definitions of what
they are, what they contain, or who they support. Lack of clarity causes confusion for DCP developers and data consumers
about how the data and metadata in the DCP are structured and organized.

### User Stories

1. As a tool developer, I would like to identify and get raw data in the DSS so that I can run my data processing and
   analysis tools.

1. As a tool developer, I would like to identify and get alignment results and count matrices in order to produce
   expression matrices.

1. As a computational biologist, I would like to get expression matrices for the Immune Cell Atlas project so that I can
   do my research.

1. As an external user of the DSS who is developing analysis tools, I would like to browse, query, and get raw and
   processed data based on some rough criteria so that I can understand what the data are and develop my tool.

1. As an Azul or Query Service developer I want to be able to distinguish between different types of bundles in order to
   know which bundle to index, link, etc. or to provide bundle types to users as a search criterion.

1. As a computational biologist, I would like to use the same references that the HCA DCP uses in their analysis.

## Detailed Design

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

| Bundle Type              | Example `bundle-type` value                                 |
|--------------------------|-------------------------------------------------------------|
| Project Metadata Bundle  | `hca/project`                                               |
| Primary Sequence Bundle  | `hca/primary-data; hca-data-type:SmartSeq2`                 |
| Primary Imaging Bundle   | `hca/primary-data; hca-data-type:sptx`                      |
| Secondary Analysis Bundle| `hca/analysis-output; hca-pipeline:snap-atac`               |
| Reference Resource Bundle| `hca/analysis-reference; hca-resource-type:reference-genome`|
| Expression Matrix Bundle | `hca/analysis-output; hca-resource-type:expression-matrix`  |

### Unresolved Questions

- Bundle degrees: Useful? Who assigns them? Who uses them?
- Is there actually added value to not having to update all bundles when e.g. the project is updated?
- Versioned/unversioned references?

### Prior Art

[Submission to bundles SOP](https://docs.google.com/document/d/1x8mYLU8ubpZtTrzkwJrft1heqReX-pJLjJfb2X0fd2w) (Apr 2018)
[Bundle metadata schema update ticket](https://github.com/HumanCellAtlas/metadata-schema/issues/986)

### Alternatives

- Status Quo: no well-defined bundle types. Existing DCP components continue to infer bundle types via ad hoc methods
  and included metadata.
