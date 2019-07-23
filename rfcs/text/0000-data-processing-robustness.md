### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# RFC Name

Data processing robustness and integrity

## Summary

This document describes requirements for individual DCP components related to robust against incorrect, corrupt, or unexpected data.  It also lays out expectations for data integrity and error handling for DCP software.  If does not specify implementation details or specific technology.

## Author(s)

[Mark Diekhans](mailto:markd@ucsc.edu),
[Tony Burdett](mailto:tburdett@ebi.ac.uk)

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

As with most biological data processing systems, the HCA DCP is a data-driven where the input is generated from external sources with a high amount of variability.  This poses challenges for processing software that need to handle problematic data in a predictable manner.

Metadata describes and is part of the data, not part of the system APIs.  Metadata  will change independently of processing software as different types of experiments are consumed and new requirements are developed.  In addition to format changes in the metadata,  it may differed in other ways, such as graph structure, from the assumptions of processing software.

*TODO: add data integrity motivation*


### User Stories

- As a DCP developer, I don't want to make urgent fixes to software due to differences or errors in data, so development can be managed in a focused manner.
- As a DCP submitter, I want to get data ingested in a timely manner, without waiting for DCP developers to make modifications to handle my experiment, so that my lab can move on to other tasks.
- As a data consumer, I don't want to receive incorrect data caused by developers attempting to work around problematic data, so that I can spend my time on actual research.

## Detailed Design

As part of normal operations, all components in the system are responsible for:
1. Skipping experimental data when it is erroneous or unexpected rather than failing.
1. Capturing these errors in a way the they can be detected and corrected.
1. Reprocess failed data when the problems are corrected.
1. Ensure completeness and consistency of data produced.

This requirements can be summarized as *log and continue* and *recover after
repair*.


*Explain the design in sufficient detail such that the implementation and (if appropriate) the interaction with existing DCP software are both reasonably clear.*

### Unresolved Questions

1. How to evaluate conformance??

- *What aspects of the design do you expect to clarify further through the RFC review process?*
- *What aspects of the design do you expect to clarify later during iterative development of this RFC?*

