### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# RFC Name

Data processing robustness and integrity

## Summary

This document describes requirements for individual DCP components related to robust against incorrect, corrupt, or unexpected data.  It also lays out expectations for data integrity and error handling for DCP software.  It does not specify implementation details or technology.

## Author(s)

[Mark Diekhans](mailto:markd@ucsc.edu),
[Tony Burdett](mailto:tburdett@ebi.ac.uk)

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

As with most biological data processing systems, the HCA DCP is driven by input generated from external sources.  This data can be highly variable as well as error-prone. This poses challenges for processing software that need to handle problematic data in a predictable manner.

Metadata is part of the data, not part of the system's APIs.  Metadata will change as different types of experiments are consumed and new requirements are developed.  In addition to format changes in the metadata,  it may differ in other ways, such as graph structure, from the assumptions of processing software.  These differences are not necessarily described by the metadata schema.

Data integrity for biological data sets goes beyond ensuring the integrity of individual files.  It involves the consistency of all data in a data set.  The partitioned nature of many single-cell experiments, along with the continuous processing design of the DCP, makes the definition of a *complete data set* difficult.


### User Stories

- As a DCP developer, I don't want to make urgent fixes to software due to differences or errors in data, so development can be managed in a focused manner.
- As a DCP submitter, I want to get data ingested promptly, without waiting for DCP developers to make modifications to handle my experiment, so that my lab can move on to other tasks.
- As a DCP data consumer, I don't want to receive incorrect or incomplete data caused by developers attempting to work around problematic data, so that I can spend my time on actual research.

## Detailed Design

As part of normal operations, all components in the system are responsible for:
- Skip process experimental data when it is erroneous or unexpected rather than failing.
- Capturing errors in a way they can be analyzed and corrected.
- Reprocess failed data when the problems are corrected.
- Ensure the completeness and consistency of data produced.

These requirements can be summarized as:
1. *log and continue*
2. *recover after repair*
3. *define completeness* 


### Unresolved Questions

- *What aspects of the design do you expect to clarify further through the RFC review process?*
- *What aspects of the design do you expect to clarify later during iterative development of this RFC?*

