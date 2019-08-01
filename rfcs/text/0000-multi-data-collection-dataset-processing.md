### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# Processing Datasets that Span Multiple Bundles

## Summary
This RFC proposes a solution to allow processing of datasets that span multiple bundles. It addresses the general problem that the current DCP data model contains no representation of data set completeness beyond that of individual data bundles.

By providing a general method for grouping bundles, this proposal provides a mechanism for addressing various tasks that cross bundle boundaries.  This impacts flexible analysis pipelines, algorithmic complexity, data consistency, and data set quality control.

The current bundle grouping under consideration is a submission to a project.  However, the concept generalizes in a manner that other groupings can be defined to accomodate future needs.

## Author(s)
- [Nick Barkas](mailto:barkasn@broadinstitute.org)
- [Mark Diekhans](mailto:markd@ucsc.edu)

## Shepherd
-[Nick Barkas](mailto:barkasn@broadinstitute.org)

## Motivation
The current DCP model of processing has a scope of one bundle, which contain a single assay or analysis.  There is no mechanism to know when a group of bundles that need to be processed together have been ingested or to trigger that processing.  This restriction that data processing is linear and based on ingestion packaging causes multiple problems, as outlined in this section.

#### Multi-input analysis 
Analysis pipelines may require input from multiple different assays. In order to perform such analyses, they must be triggered only when all input data is available. The current mechanism of bundle notifications is not effective when data spans multiple bundles because there is no clear set of rules to identify the bundles that need to be co-processed.  Limiting the downstream process to the scope of a single bundle requires the ingest process to define the scope of the processing without any foreknowledge of future processes needs.

An immediate need to for analysis pipelines to process 10X V2 scRNA-seq datasets that span multiple bundles exists. The need for co-processing multiple bundles is not however limited to this scenario and extends to any other NGS modality that involves repeated sequencing of the same library. Furthermore, the need to co-process data may extend to other data modalities that the project will accept in the future.

In the future, there may be a need to run analysis processes where input sets span multiple projects. A mechanism to specify processing data collections that are not restricted to a single project or submissions will be required to implement this type of analysis.

#### Algorithmic complexity and performance
Producing combined metadata or data from multiple assay bundles using the bundle event system results in O(N^2) algorithmic complexity.  As each bundle event arrives, it must be combined with accumulated metadata from previous bundles.  Additionally, there is a race condition on reading and writing the accumulated the results that must be handled.

This impacts the data browser creating project metadata TSVs and normalized JSON files, and the matrix service creating pre-built project matrices.

#### Data consistency
Consistency and completeness across a data set is an important attribute of an atlas.  An incomplete data set may result in misinterpretation of data or missed discoveries.  Currently, the DCP has no way to indicate data completeness beyond a bundle. The consumer API and data browser don't have the knowledge to indicate a project submissions is incomplete.  This information is only available within ingest, with no way to communicate submission status to other components.

#### Data set quality control
An approach to quality control (QC) is to run a partial or full analysis on an subset of the data before doing a full analysis.  This requires defining an analyzable subset of the data to pass on to the processing pipelines. A QC data subset must meet the criteria to that allows the QC analysis to run.

## User Stories
As a data contributor, I want to maintain flexibility in the laboratory approaches I can use to perform top-up sequencing, library multiplexing and arrange replicates on flowcells in a way that minimizes overall cost, while being assured that the processing will be correct.

As a data-wrangler, I want to be able to succinctly enter the metadata I receive from the data contributor and have a clear understanding of what I need to do to ensure correct processing.

As a schema developer, I want to ensure we are capturing and requiring, the metadata from a contributor to ensuring the pipelines can run appropriately.

As a member of the pipelines team, I want to ensure that the pipelines process the correct data sets and I can easily access the metadata required to trigger the pipelines.  I do not want to be restricted in designing analysis based on how data was submitted.

As a data consumer, I want to be able to find all data files that need to be processed together so that I can run my data processing pipeline. I do not want to be restricted in designing analysis based on how data was submitted.

As a data consumer, I want to be confident that the HCA DCP has correctly processed all data files that belong together so that I know the matrices I receive have been generated correctly.

As a data consumer, I need to know that a data set is incomplete, so that I don't download and use it until it is complete.

## Detailed Design
A new concept of *data group* is added to the DCP data model to address these issues.  A data group is defined as a set of specific versions of metadata and data that is annotated as complete and consistent by specified criteria.  Events are generated when a *data group* is created, updated, or deleted.

A data group has a symbolic scope type that specifies what is represented by the group, as well a the set of criteria by which it is complete.  Most of the above use cases will requires a *project submission* scope that is indicates a submission to a project is complete.

Data groups are implemented as a new bundle type that contains a JSON file listing the FQIDs (UUIDs with versions) of all bundles that part of the data groups.

A new schema *data_group* will be created that contains the fields:
- *scope* - Symbolic name of the scope, for example *PROJECT_SUBMISSION*.
- *bundle_fqids* - List of FQIDs of bundles that are in the group.

The *data_group* implementation shall be flexible so as to allow addition of further fields for future use without breaking current functionality.

Ingest is responsible for creating *data groups* after the relevant bundles have been created. A *data group* that references bundles that do not exist is invalid and analysis is not required to process it.

[TODO add diagram here]

#### Proposed *data group* scopes
Different *data group* scopes will have different life-cycles, processing requirements, as well as criteria for completeness. We initially propose the use of two types of scopes. Further types can be added in the future as required.

*PROJECT_SUBMISSION* scope: *Data groups* of scope *PROJECT_SUBMISSION* will indicate that the entirety of a single project submission is complete as indicated by the submitters and all data in the project submission should be processed by downstream components.

*QUALITY_CONTROL* scope: The QC scope indicates that a small dataset that will form part of the project submission is to be processed in order to assess data quality. The resulting bundle should be marked so as to indicate that it is not complete and only to be used for QC purposes. Analysis bundles resulting from QUALITY_CONTROL scoped *data sets* should not be displayed on the browser as regular data sets.

### Updates to *data groups*
Updates to PROJECT_SUBMISSIONS must result in updating of the associated *data groups* in order to trigger reprocessing. When part or the entirety of a project submission is updated the associated data group must be identified and updated with the new bundle versions, if bundles have been replaced and/or with additional new bundles if bundles have been added. This is the responsibility of Ingest. Analysis must handle the update to the *data group* by initiating only the pipelines that have been affected by the update and ensuring that the output bundles of these analysis are updates to existing bundles.

### Notification Requirements
In order for the *data group* to be actionable by analysis, it must fulfill specific requirements. Specifically, the notification must provide a list of *bundle_fqids* that belong to the same sequencing set and _potentially_ need to be co-processed.

The *data group* will not force co-processing of data, it will merely indicate that data to be co-processed should be identified within the *data group* and that that group fulfills completeness requirements as per the *data group* type. Co-processing will be initiated by pipelines on the basis of a well defined and pre-agreed ruleset that is modality specific.

The imposition of specific requirements to metadata is part of this RFC proposal and a key component of the proposed *data group* solution.

This approach provides a generalized signaling mechanism suitable for all foreseeable data types while accounting for data type-specific requirements.

## Metadata Requirements
When analysis receives an incoming *data group* it needs to identify how many pipelines it needs to launch and which bundles should be included in each analysis. This is because the *data group* only indicates that the groups of bundles needs to be _potentially_ co-processed, i.e. that co-processing is to be done within the given set of files.

Analysis can initiate multiple pipeline invocations as a result of a single *data group* being submitted, depending on what actually needs to be co-processed within the *data group* and what pipelines are available. For example if a *data group* contains multiple SSII bundles that are all part of a single plate a pipeline for plate processing exists, only one pipeline invocation per plate would be initiated. If only pipelines for one cell anaysis exist then one pipeline invocation per run will be actuated.

The actual grouping of the files is to be performed by analysis via a search in the metadata of the bundles submitted following well-defined modality-specific set of rules. Here we provide an abstract algorithm and defined metadata requirement (MRs) that will allow analysis to identify the pipelines that need to be triggered. The metadata requirements are as follows:

- MR1: The data modality of each data file included in the *data group* must be clearly identifiable (i.e. must be able to tell if the data is 10X, SS2 or of some other data modality)
- MR2: Data that need to be processed together must be clearly identifiable by a modality-specific set of metadata variables that will group the files according to processing requirements
- MR3: Among the data that needs to be processed together, the metadata must provide sufficient information to correctly order, pair or otherwise establish correspondence and type of the input files. (e.g. in 10X identify R1, R2 and I1 triplets)

## Example: Analysis Implementation for a *data group* 
This section outlines an example implementation for a *data group* through-out it's lifecycle.

[ Consider a figure here ]

- The wranglers prepare a submission and submit via ingest
- Ingest places the data in data bundles
- Data bundle notifications are fired but analysis does not listen to these events
- After all the data is confirmed to be available and correct a *data set* bundle is created in the DSS of scope *PROJECT_SUBMISSION* is created. This consititues the signal for analysis initiation
- Analysis receives a bundle notification for the *data set*
- Analysis recovers the metadata for all the bundles in the *data set*
- Analysis maintains a list of registed *data set* handlers and executes each handler (this is for future extensibility)
- The default handler executes as follows:

```
Partition the datasets by data modality (MR1)
For each data modality:
    Use the modality-specific criteria to identify pipeline invocations (MR2)
    For each invocation:
        order the data as required (MR3) and launch pipeline invocation
```

Note that the above approach does NOT require all the data in a submitted *data group* to be of the same modality to be processed correctly. For example a *data group* at the project level can contain imaging and 10X datasets and these will be correctly processed in an independent manner, by initiating separate pipeline invocations. This satisfies an existing need, for example the dataset named "Reconstructing the human first trimester fetal-maternal interface using single cell transcriptomics" contains data of different modalities.

Furthermore if part of the data submitted in a *data group* has already been processed the Analysis infrastructure can only re-process the parts that have not been modified. The outputs of each pipeline must reference the *data group* that triggered tha analysis of the given set for providence purposes.

## Metadata Requirements for 10X V2 3’ scRNA-seq datasets
The specific implementation of the above metadata requirements for 10X V2 3’ RNA-seq datasets is presented here as an initial use case and an example. 10X V2 3’ data are the most common type of data currently deposited in the DSS and therefore represent a widely applicable use case.

Requirement (1) above can be satisfied by having a globally unique ‘Library ID’ variable in the metadata that identifies the Illumina sequencing library from which each input file originates from. More details on the requirements and a specific implementation of this solution can be found currently under consideration here: [PR](https://github.com/HumanCellAtlas/dcp-community/pull/87), [document](https://github.com/HumanCellAtlas/dcp-community/blob/mfreeberg-rfc-library-preparation/rfcs/text/0000-rfc-library-preparation.md).

Requirement (2) can be satisfied by a globally unique triplet identifier.

## Example 2: Metadata Requirements for SS2 scRNA-seq datasets and provision for plate-based analyses
The above metadata requirements are also compatible with SS2 analyses and can support plate based-analyses. Cells that have been sequenced more than once (a rare, but plausible scenario for SS2) can be identified by using a library identifier as presented in this RFC, currently under review.

Furthermore, this approach can be extended to support plate-based processing. By ensuring that a plate field is provided for each cell, multiple cell bundles can be aggregated into a single plate-based workflow. A *data group* can contain one or more plates. Provision must be made for any cells not associated with a plate identifier, or otherwise, the plate identifier must be a compulsory field.

### Unresolved Questions
*   How does this framework interact with new modalities we are likely to encounter in the future. A good example of this is imaging data? (Feedback by Ambrose Requested)
*   Link to [Library Prep RFC)(https://docs.google.com/document/d/1PdJAd4q775jwnDoavJE_7XajXm0e7a5SGvP2k7DRJHc/edit#heading=h.5ildwmh2beeb)
*   Possible alternative/Synergistic solution: project or dataset bundles with rigidly defined bundle types
*   Possible alternative/Synergistic solution: bundle lifecycle (unpublished/in progress vs. published/finished bundles)

## Definitions
**Data modality**: A type of data in the HCA, for example NGS scRNA-seq or a particular type of imaging.

**Unique Library Identifiers**: A unique identifier (string or numeric) that globally identifies each library preparation for sequencing data.

**Co-processing (of data)**: Providing data as input to a single pipeline invocation

**MR**: Metadata Requirement. The term refers to specific metadata requirements for bundles to be able to process in analysis.
