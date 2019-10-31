### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# RFC DCP data processing robustness principles

## Summary

This document describes requirements for individual DCP components related to robustness against incorrect, corrupt, or unexpected data.  It also lays out expectations for data integrity and error handling for DCP software.  It does not specify implementation details or technology.

## Author(s)

[Mark Diekhans](mailto:markd@ucsc.edu),
[Tony Burdett](mailto:tburdett@ebi.ac.uk)

## Shepherd
[Tony Burdett](mailto:tburdett@ebi.ac.uk)

## Motivation

The HCA DCP is driven by input generated from external sources. As most developers on biological data processing systems can attest, this produces data that is highly variable and error-prone. Collecting, storing and processing this type of content can pose challenges for software, as systems must be highly robust against all possible types of problematic data. When unexpected content is encountered, it needs to be handled reliably and predictably.

As well as capturing data (e.g. sequencing FASTQ files, sequence alignment BAM files, images), the HCA DCP contains experimental "metadata". This metadata precisely describes biological materials, experimental processes, and protocols, and as such is considered by most scientists in biomedical research to be a source of [data about an experiment](https://en.wikipedia.org/wiki/Metadata#In_biomedical_research). This is different from how many software engineers consider metadata, for example [on the internet](https://en.wikipedia.org/wiki/Metadata#On_the_Internet) or in [digital media](https://en.wikipedia.org/wiki/Metadata#In_broadcast_industry). Critically, DCP experimental metadata is part of a searchable, informational record and has value in its own right, distinct from merely annotating data files. As such, it is important the DCP can adequately handle and tolerate many differences in its metadata, rather than treat metadata as part of a static API description.

The single-cell community is part of a new field, and the optimal experimental designs are currently poorly understood. This leads to high levels of variability in the experiments being conducted, and therefore in the descriptions of experimental techniques and technologies used. This, in turn, creates highly variable and highly volatile metadata, both in terms of the experimental design (which is typically expressed as a graph capturing experimental steps) and in terms of the descriptive elements required (captured in the metadata schema itself). Over time, experimental metadata in the DCP will, therefore, represent a wide range of variability of experiment designs and schema versions. This variability will be many orders of magnitude higher than the variability observed in the types of data captured by the DCP.

Likely, most consumers of DCP data will only be interested in some of the experimental metadata. In some cases, this will be confined to a subset of the available metadata (e.g. "which dissociation protocol was used in this experiment?") and in other cases, it will be confined to certain experimental designs ("how was the organoid created in this experiment?"). In both scenarios, we will observe differences. Over time, the set of fields needed to adequately define dissociation protocols will likely change, and not all experimental designs make use of organoids.

Given the manually created nature of experimental metadata, it's high variability, and the needs of consumers, it is likely that errors and mismatches will occur. Assumptions of data consumers will be invalid, highly unusual experimental designs that had never been considered when writing software components will be submitted, and metadata that is not backward compatible or simply incorrect, will all end up in the DCP.

Data integrity for biological data sets goes beyond simply ensuring the integrity of individual files, however. All files in a coherent data set (e.g. all FASTQ files from a single sequencing experiment) must be valid to the same standard, and the experimental metadata that references these files must correctly describe which files were generated from which technique, for example. The partitioned nature of many single-cell experiments, along with the continuous processing design of the DCP, makes it hard to even define a *complete data set*, let alone ensure it's integrity.

To create a robust and resilient system, it is critical that DCP components are engineered with a liberal attitude to data and experimental metadata, being highly tolerant to errors in data and metadata, integrity problems, and unexpected content. This is likely to mean that DCP components encounter data that they cannot handle, and this is an acceptable compromise as long as these cases are highly visible. This RFC proposes several strategies to engineer data processing systems for robustness and data integrity.

### User Stories

- As a DCP developer, I don't want to make urgent fixes to software due to differences or errors in data, so that development efforts can be planned and managed in a controlled manner, rather than a reactive one.
- As a DCP submitter, I want my data to be ingested promptly, without waiting for DCP developers to make modifications to handle my experiment, so that my lab can move on to other tasks.
- As a DCP data consumer, I don't want to receive scientifically incorrect, inaccurate or incomplete data caused by developers attempting to work around problematic data, so that I can spend my time on actual research.

## Detailed Design

In this RFC, we require two main strategies for making changes across the DCP that will ensure greater reliability and predictability of data processing. These strategies will reduce coupling between components, providing the opportunity for development teams to react in a controlled, planned manner whenever data is encountered that fails to meet the assumptions or expectations of DCP software components.

The proposed strategies are:
1. *Log and Continue*
2. *Repair and Recover*

### Log and Continue

- DCP components define their expectations of any data they receive (see 
[Querying DSS by Metadata Schema Version(s)](https://github.com/HumanCellAtlas/dcp-community/blob/master/rfcs/text/0011-query-by-metadata-schema-versions.md)
- DCP components ignore data they retrieve if it is mismatched against their expectations (e.g. bundles contain unrecognised file formats such as PDFs)
- DCP components skip processing of data and fail gracefully when they encounter an error, rather than crashing or producing incorrect results.
- DCP components log all errors in a manner that allows them to be detected and addressed.  See [Monitoring for production systems](https://github.com/HumanCellAtlas/dcp-community/blob/master/rfcs/text/0002-monitoring-for-production-systems.md).
- DCP components report correct error statuses (e.g. via HTTP response codes) to clients as appropriate.


### Repair and Recover

- DCP components include operator admin functions to manually trigger automated steps that may have failed
- In case of errors that causes some data to be skipped, DCP components ensure that all data expected to be handled together (e.g. all data from a single project) complete before making some of the data available


### Unresolved Questions

- It is expected that this RFC to raise questions that require more detailed specification of the requirements.
- How each component determines and reports compliance to these principles needs to be defined.

### Prior work:
- [The Harmful Consequences of the Robustness Principle](https://tools.ietf.org/html/draft-iab-protocol-maintenance-03) - Despite the name, this IETF draft very much supports a premise of this RFC, which is to not attempt process unexpected data, rather skip and move on in a principled manner.
