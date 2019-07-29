### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# Processing Datasets that Span Multiple Bundles

## Summary
This RFC proposes a solution to allow processing of datasets that span multiple bundles. It address the general problem that the current DCP data model contains no representation of data set completeness beyond individual bundles.

By providing a general method for grouping bundles, this proposal provides a mechanism for addressing various task that cross bundle boundaries.  This impacts flexible analysis pipelines, algorithmic complexity, data consistency, and data set quality control.

The current bundle grouping under consideration is a submission to a project.  However, the concept is generalize in a manner that other groupings can be defined.


## Author(s)
[Nick Barkas](mailto:barkasn@broadinstitute.org),
[Mark Diekhans](mailto:markd@ucsc.edu)

## Shepherd
-[Nick Barkas](mailto:barkasn@broadinstitute.org)

## Motivation
The current DCP model or process has a scope of one bundle, which contain a single an assay or analysis.  The is no mechanism to know when a group of bundles that need to be processed together have been ingested or to trigger that processing.  This restriction that data processing is linear and independent based on ingestion packaging causes multiple problems,
as outline in this section.


### Multi-input analysis 
Analysis pipelines may require input from multiple different assays. In order to run such analysis, they must be triggered while all input data is available.  The current mechanism of bundle notifications is not effective when data spans multiple bundles because there is no clear set of rules to identify the bundles that need to be co-processed.  Limiting the downstream process to the scope of a single bundle requires the ingest process to define the scope of the processing without any foreknowledge of future processes needs.

An immediate need to for analysis pipelines to process 10X V2 scRNA-seq datasets that span multiple bundles exists. The need for co-processing multiple bundles is not however limited to this scenario and extends to any other NGS modality that involves repeated sequencing of the same library. Furthermore the need to co-process data may extend to other data modalities that the project will accept in the future.

In the future, there maybe a need to run analysis processes that were input sets span multiple projects.  A mechanism to specify processing data collections that are not restricted to a single project or submissions will be required to implement this type of analysis.

### Algorithmic complexity and performance

Producing combined metadata or data from multiple assay bundles using the bundle event system results in O(N^2) algorithmic complexity.  As each bundle event arrives, it must be combined with accumulated metadata from previous bundles.  Additionally, there is a race condition on reading and writing the accumulated the results that must be handled.

 This impacts the data browser creating project metadata TSVs and normalized JSON files, and the matrix service creating pre-built project matrices.

*[TODO: is the query service impacted?]*


### Data consistency

Consistency and completeness across a data set is an important attribute of an atlas.  An incomplete data set may result in misinterpretation of data or missed discoveries.  Currently,
the DCP has no way to indicate data completeness beyond a bundle. The consumer API and data browser don't have the knowledge to indicate a project submissions is incomplete.  This information is only available within ingest, with no way to communicate submission status to other components.

### Data set quality control

An approach to quality control (QC) is to run a partial or full analysis on an subset of the data before doing a full analysis.  This requires defining an analyzable subset of the data to pass on to the processing pipelines. A QC data subset must meet the criteria to that allows the QC analysis to run.

## User Stories

As a data contributor, I want to maintain flexibility in the laboratory approaches I can use to perform top-up sequencing, library multiplexing and arrange replicates on flowcells in a way that minimizes overall cost, while being assured that the processing will be correct.

As a data-wrangler, I want to be able to succinctly enter the metadata I receive from the data contributor and have a clear understanding of what I need to do to ensure correct processing.

As a schema developer, I want to ensure we are capturing and requiring, the metadata from a contributor to ensuring the pipelines can run appropriately.

As a member of the pipelines team, I want to ensure that the pipelines process the correct data sets and I can easily access the metadata required to trigger the pipelines.  I do not want to be restricted in designing analysis based on how data was submitted.

As a data consumer, I want to be able to find all data files that need to be processed together so that I can run my data processing pipeline.   I do not want to be restricted in designing analysis based on how data was submitted.

As a data consumer, I want to be confident that the HCA DCP has correctly processed all data files that belong together so that I know the matrices I receive have been generated correctly.

As a data consumer, I need to know that a data set is incomplete, so that I don't download and use it until it is complete.

## Design

A new concept of *data group* is added to the DCP data model to address these issues.  A data group is define as a set of specific versions of metadata and data that is defined as complete and consistent in by a specified criteria.  Events are generated when a *data group* is create, updated, or deleted.

A data group has a symbolic scope type that specifies what is represented by the group.  Most of the above uses cases will requires a *project submission* scope that is indicates a submission to a project is complete.

Data groups are implemented as a new bundle type that contains a JSON file listing the FQIDs (UUIDs with versions) of all bundles that part of the data groups. 

A new schema *data_group* will be create that contains the fields:
- *scope* - Symbolic name of the scope, for example *PROJECT_SUBMISSION*.
- *bundle_fqids* - List of FQIDs of bundles that are in the group.

[TODO add diagram here]


* TODO: 
- How updates to project submissions work data groups with needs to be defined.
- Are analysis submissions a different project scope than primary data submissions?



## Example 
*[MODIFY AND SIMPLIFY THE BELOW TEXT TO BE AN EXAMPLE]*


### Definitions

**Unique Library Identifiers**: A unique identifier (string or numeric) that globally identifies each library preparation for sequencing data.

**Co-processing (of data)**: Providing data as input to a single pipeline invocation

**MR**: Metadata Requirement. The term refers to specific metadata requirements for bundles to be able to process in analysis.

### Component Notification Changes
[this implies that DAPS is not a bundle]
This RFC proposes no longer using bundle-level search-based notifications on primary bundles for initiating pipelines. Instead, we propose that a **specific trigger** is used to initiate analysis pipelines. This document lists the requirements for such notifications to be actionable by analysis and makes recommendations on overall implementation paradigm with specific examples for 10X and SS2 datasets.

### Notification Requirements
For a notification to be actionable by analysis, it must fulfill specific requirements. Specifically, the notification must include information about the bundles/data that belong to the same sequencing set and potentially need to be co-processed.

We propose the term **Data Aggregation and Processing Set (DAPS)** to encompass all the different alternative representations of such sets. The only requirement imposed on such these representations is to be reproducible and resolvable to an immutable sets of data. Example representations of DAPS can be:

*   A listing of all relevant bundles along with versions
*   A search query accompanied by a timestamp (to guarantee reproducibility and immutability)
*   Other future representations as project needs evolve (e.g. linking to a particular project bundle version)

Critically, the submitted DAPS will not explicitly dictate the files that have to be co-processed in pipeline instances as this would require specialized knowledge of the pipelines by the submitter. Instead, **a DAPS indicates that aggregation of data must be performed only from the list of data provided (submission envelope), but the actual aggregation is delegated to the process of analysis initiation, subject to well defined modality-specific metadata requirements**. 

The imposition of specific requirements to metadata is part of this RFC proposal and a key component of the proposed DAPS solution.

This approach provides a generalized signaling mechanism suitable for all foreseeable data types while accounting for data type-specific requirements.

The DAPS approach has common elements with project-level bundles that have been proposed in other contexts, but differs in the following ways:

1. It does not impose any specific aggregation level on the data allowing for more flexibility and accommodating future unforeseen processing scenarios
2. Allows specification of bundles included in a DAPS in many alternative different ways (listing, search, etc…)
3. Does not impose the creation of a new bundle type that is tied to a specific data modality
4. Dictates specific metadata requirements that guarantee the correct processing of all foreseeable data modalities
5. Decouples analysis initiation from data representation allowing for future flexibility

## General Metadata Requirements
In the above framework, analysis pipelines will receive a set of bundles that _potentially_ need to be co-processed. As aforementioned, this signal will not explicitly identify the bundles that need to be co-processed in a single pipeline, instead, it will serve as a notification **indicating that any co-processing is to be done only within the boundaries of this set of files (submission envelope). **

Analysis can initiate multiple pipeline invocations as a result of a single DAPS being submitted.

The actual grouping of the files is to be performed by analysis via a search in the metadata of the bundles submitted following well-defined modality-specific set of rules. For this reason, the metadata must satisfy the following metadata requirements (MR):

Furthermore, if part of the data submitted in a DAPS has already been processed the Analysis infrastructure will only re-process the parts that have not been modified.

1. The data modality of each data file included in the DAPS must be clearly identifiable (MR1)
2. Data that need to be processed together must be clearly identifiable by a modality-specific set of metadata variables that will group the files according to processing requirements. (MR2)
3. Among the data that needs to be processed together, the metadata must provide sufficient information to correctly order, pair or otherwise establish correspondence and type of the input files. (MR3)

## Proposed Signalling Implementation
The DAPS proposal above only sets specific requirements for the signal to initiate data analysis and minimum requirements for the associated metadata. We hereby provide a specific recommendation for implementation via bundle notifications.

We recommend that the data store (DSS) is used to store DAPS as a new bundle type of **“DAPS Manifest”**, and that the current search-based notification system is adjusted so that instead of listening for new primary bundles, analysis listens for new DAPS bundles that are exclusively used for the initiation of pipelines. This implementation has two major advantages: (1) the DAPS are permanently saved at the main storage site for the HCA project (DSS) so the analyses can be reproducibly triggered and (2) The existing notification mechanisms are reused, avoiding the need for implementation of a new out-of-band notification system.

We propose that as an initial implementation approach DAPS are used at set data aggregation levels (e.g. sample, project) but critically the implementation must be flexible to arbitrary data aggregation levels to accommodate future project needs. We propose that triggering is initially done manually by data wranglers and/or ingest, but we envisage transition to automatic triggering in the future.

## Example: Analysis Implementation for a DAPS
The following pseudocode presents a proposed implementation of the processing of an incoming DAPS by analysis:
```
Partition the datasets by data modality (MR1)
For each data modality:
    Use the modality-specific criteria to identify pipeline invocations (MR2)
    For each invocation:
        order the data as required (MR3) and launch pipeline invocation
```

Note that the above approach does not require all the data in a submitted DAPS to be of the same modality to be processed correctly. For example a DAPS at the project level can contain imaging and 10X datasets and these will be correctly processed in an independent manner. 

Furthermore if part of the data submitted in a DAPS has already been processed the Analysis infrastructure will only re-process the parts that have not been modified.

## Metadata Requirements for 10X V2 3’ scRNA-seq datasets
The specific implementation of the above metadata requirements for 10X V2 3’ RNA-seq datasets is presented here as an initial use case and an example. 10X V2 3’ data are the most common type of data currently deposited in the DSS and therefore represent a widely applicable use case.

Requirement (1) above can be satisfied by having a globally unique ‘Library ID’ variable in the metadata that identifies the Illumina sequencing library from which each input file originates from. More details on the requirements and a specific implementation of this solution can be found currently under consideration here: [PR](https://github.com/HumanCellAtlas/dcp-community/pull/87), [document](https://github.com/HumanCellAtlas/dcp-community/blob/mfreeberg-rfc-library-preparation/rfcs/text/0000-rfc-library-preparation.md).

Requirement (2) can be satisfied in two alternative ways: (a) sufficient metadata can be provided as outlined in table 1 below to identify all file triplets, or (b) a globally unique triplet field can be used to identify file correspondence by the submitter. We propose the latter approach as collecting all the required metadata may not be feasible in all cases and places an undue burden on the submitter in the common scenarios where only one triplet is submitted.

We propose that the metadata required to automatically identify the triplets (Table 1) is made optional and users are encouraged to submit these fields as it can help identify errors and also can in some cases provide important covariates for scientific analyses.

**Table 1: Fields required for identification of fastq file correspondence**
<table>
  <tr>
   <td><strong>Required Fields</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Justification</strong>
   </td>
  </tr>
  <tr>
   <td>Run Date and Time
   </td>
   <td>ISO Date and time at which the run was performed
   </td>
   <td>This is for the rare scenario where flowcells are re-used and ensures that each run is processed as if it were a separate flowcell
   </td>
  </tr>
  <tr>
   <td>Flowcell ID
   </td>
   <td>The Illumina identifier of the flowcell id
   </td>
   <td rowspan="3" >The unique combination of Flowcell ID, Lane and Barcode uniquely identifies any multiplexed library on an Illumina Flowcell
   </td>
  </tr>
  <tr>
   <td>Lane in Flowcell
   </td>
   <td>The lane in the flowcell
   </td>
  </tr>
  <tr>
   <td>Barcode of Library
   </td>
   <td>Barcode (or concatenation of the barcodes) of the library
   </td>
  </tr>
</table>

## Metadata Requirements for SS2 scRNA-seq datasets and provision for plate-based analyses
The above metadata requirements are also compatible with SS2 analyses and can support plate based-analyses. Cells that have been sequenced more than once (a rare, but plausible scenario for SS2) can be identified by using a library identifier as presented in this RFC, currently under review.

Furthermore, this approach can be extended to support plate-based processing. By ensuring that a plate field is provided for each cell, multiple cell bundles can be aggregated into a single plate-based workflow. A DAPS can contain one or more plates. Provision must be made for any cells not associated with a plate identifier, or otherwise, the plate identifier must be a compulsory field.

## Pros and Cons of DAPS Approach

Pros
*   Avoids potentially very expensive run-away queries
*   Ensures that analyses are triggered once with the appropriate datasets
*   Is flexible and can accommodate all foreseeable data modalities
*   Can immediately resolve processing challenges with 10X datasets
*   Can provide a mechanism for moving to plate-based processing for SS2 in a seamless manner
*   Does not preclude future automation of pipeline initiation when a mature system state is achieved
*   Re-uses existing notification infrastructure
*   Is permanently saved in the DSS and analyses are reproducible
*   Provides a natural place to provide analysis-specific parameters

Cons
*   Analysis need to modify pipeline initiation scripts
*   Initial implementation has human time cost of triggering the pipeline runs
*   Requires the human who is triggering the pipeline to know what criteria need to be satisfied to trigger (i.e. the project is complete)

## Alternative Approaches 

The following section summarizes possible alternative approaches for ensuring correct processing, along with the pros and cons of each. These approaches are not recommended by this RFC but are listed for comparison purposes.

*   Placing all the data that needs to be co-processed in a single bundle and continue to use bundle notifications
    *   Pros
        *   Essentially no extra work from infrastructure required
    *   Cons
        *   Wranglers are required to package data in such a way to satisfy the requirements of the analysis, the details of which they should not be aware
        *   Additional strain is placed on wranglers to ensure that the correct partition of the data is archived
        *   If new ways of aggregating data are needed bundles would have to be reshaped
        *   This does not support the need to be able to support multiple potential aggregations or shapes of bundles at one time
*   Use a search-based approach to identify and co-process bundles
    *   Pros
        *   Additional work limited to adding a query
        *   Uses existing query mechanisms and emerging such as query service
        *   Removes human interference and allows metadata to dictate the analysis
    *   Cons
        *   The query can be very complex and there is no easy way to verify that the results are correct in the long run
        *   Analysis has to implement a completely different approach that will allow them to keep track of which workflows have been submitted
        *   There is no direct control on when analyses are triggered, its possible that run-away queries due to errors in the metadata trigger wrong but very expensive analyses
        *   The cost of these searches will increase over time as each bundle addition will require searching the entire project
*   Multi-bundle notifications. Analysis can subscribe to notifications that there is a set of bundles that meets certain criteria (e.g. that 50 analysis bundles were expected and 50 have been received.
    *   Pros
        *   Little additional work by analysis to trigger pipelines over multiple bundles
        *   Automated solution rather than manual
    *   Cons
        *   Ingest needs to pass some information (through the bundle manifest rather than the metadata?) which says there are n bundles and this is bundle i of n.
        *   Unclear how this would work with updates. If 1 bundle of 50 needs to be updated then how do we know to retrigger the notification?
        *   Data store/query service/someone else will need to implement a multi-bundle notification
*   Ingest notifies. For a particular submission, ingest knows how many bundles are generated and when they go in the data store. In principle, ingest could generate a notification for a group of bundles ingested together
    *   Pros
        *   Not too much additional work by analysis to trigger pipelines over multiple bundles (receive notification from ingest rather than through the datastore)
        *   Automated solution rather than manual
        *   No need for additional bundle manifest information or metadata in the bundles
    *   Cons
        *   Unclear how this would work with updates. If 1 bundle of 50 needs to be updated then how do we know to retrigger the notification?
        *   Ingest needs a new notification mechanism and declarations from components as to what will be a trigger. Some duplication of data store notifications.
        *   Ingest ends up talking directly to analysis and future non-primary components where this doesn’t happen currently. Complicates system architecture.

### Unresolved Questions

- *What aspects of the design do you expect to clarify further through the RFC review process?*
- *What aspects of the design do you expect to clarify later during iterative development of this RFC?*

*   How does this framework interact with new modalities we are likely to encounter in the future. A good example of this is imaging data? (Feedback by Ambrose Requested)
*   Link to [Library Prep RFC)(https://docs.google.com/document/d/1PdJAd4q775jwnDoavJE_7XajXm0e7a5SGvP2k7DRJHc/edit#heading=h.5ildwmh2beeb)
*   Possible alternative/Synergistic solution: project or dataset bundles with rigidly defined bundle types
*   Possible alternative/Synergistic solution: bundle lifecycle (unpublished/in progress vs. published/finished bundles)
