### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# HCA DCP Data Citation Plan

## Summary

Data Contributors need to be able to cite data sets that they have contributed to the DCP.
Data Consumers need to be able to cite the DCP data that they have used in their research projects.
Providing citable records for the DCP will enable all contributors and consumers to reference the data stored in the DCP that they used in their scientific publications

## Author(s)

 [Trevor Heathorn](mailto:theathor@ucsc.edu)

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

There is currently no clear and agreed upon definition of the requirements for Data Citation in the DCP.
Key issues that this RFC seeks to resolve are:
  - What is the minimum feature set required for a first release (Phase 1)?
  - What are the discrete set of features that make sense for subsequent releases (e.g. Phase 2, Phase 3, etc.)
  - In which phase, if any, is support for a formal digital object identifier ([DOI](https://en.wikipedia.org/wiki/Digital_object_identifier)) required?

### User Stories

1. As a data contributor (e.g. researcher with a pipette), it is essential to have a unique way for my project (consisting of primary and secondary analysis data files and associated metadata files) to be identified so that others can properly cite my work when using my data, and that this citation identifier is available in the DCP Data Portal. Note: This will discourage but not prevent others from using my data without attribution.
2. As a data consumer (e.g. researcher with a keyboard), I want to be able to view and share a unique citation identifier so that a reader of my manuscript can obtain the data needed to reproduce my results. Anyone can use the citation identifier to view and download all the original cited data and metadata files for a project from the DCP.
3. As a data consumer, I need a simple way to reference a project in the DCP so that I can fulfill the requirements of a Creative Commons attribution license (CC-BY).
4. As a data contributor or consumer, I need a way to use the citation identifier to access the output produced by the DCP Matrix Service for the data being cited.
5. As a data contributor or consumer, I want to be able to update my project and provide versioned views of the data being cited.

## Scientific "guardrails" [optional]

*Describe recommended or mandated review from HCA Science governance to ensure that the RFC addresses the needs of the scientific community.*

## Detailed Design

It is proposed to split the initial implementation into three phases:

### Phase 1
This is designed to satisfy the minimal set of requirements for User Stories #1 through #4 by providing only "per-project" citations.
The Data Browser project details page will add a "To cite this project please copy this link" item.
This "stable non-versioned project URL" will link back to the production site project page using the project's UUID (e.g. https://data.humancellatlas.org/explore/projects/cc95ff89-2e68-4a08-a234-480eca21ce79).
The URL refers to the “live" view of the project and is therefore subject to additions and updates (e.g. corrections) of data and metadata. However, these are expected to be infrequent and should not affect existing primary data.
If an existing project is deleted and re-ingested then the cited project UUID would become invalid. If such re-ingestion occurs then a means will be provided to redirect the original "stable project URL" to the new version of the project (e.g. by providing a landing page which states something similar to "This project has been updated with corrected data and/or analyses. For the current version of this project click *here*", where *here* is a link to the new project page).
Note: Scientists are *already* citing such project based URLs in publications.
A formal DOI is *not* required for Phase 1.

### Phase 2
This is designed to satisfy the data contributor requirements for User Story #5.
This allows the Data Operations team to make a “Data Release” (i.e. a curated data set) in which the specific versions of projects within the Data Release itself are citable.
A Data Release must be immutable.
The Data Browser must provide users with access to each Data Release (i.e. immutable version of data) in addition to the “live/latest” view.
The Data Browser must be able to provide users with a means to download the data and metadata associated with a Data Release.
Since the Matrix Service does not support the ability to process older versions of input files, the per-project output files from the Matrix Service *must* be stored as part of the immutable data set.
The Data Store "collections" API provides a suitable means for recording the contents of a Data Release.
A formal DOI is required for this phase as is provides a standardized method for specifying specific versions of data objects.

### Phase 3
This is designed to satisfy the data consumer requirements for User Story #5.
This also fully satisfies User Story #2 by allowing a data consumer to cite an arbitrary set of data which spans multiple projects.
The data consumer must be able to create a discrete collection of their selected data with the ability to update that collection (i.e. create a new version of the collection).
The Data Browser must provide users access to each version of their collections.
A data consumer must be able to share a citable DOI link to any of the collection versions that they have created.

### Implementation notes for DOI support
A DOI provides a link of the form https://doi.org/xxxx which resolves via the hosting [Registration Agency](https://www.doi.org/registration_agencies.html) or [open-access repository](https://en.wikipedia.org/wiki/Open-access_repository).
A DOI is the most commonly accepted means for citing documents and data in scientific publications.

It is an Unresolved Question as to whether a DOI may resolve to an external website (e.g. BioStudies, Zenodo, etc). Such an external web page could then provide a URL to the project in the Data Browser as well as the ability to store point-in-time copies (i.e. versions) of relevant files such as the download manifest, metadata tsv, matrix output file, etc.
The stored manifest file could then be used in the HCA CLI to download the exact versions of the data and metadata for that version of the project. It is a requirement that the DCP never deletes older versions of cited data files, except for the special case of retraction of unconsented data.

#### Possible DOI Registration Agencies and open-access repositories that provide the ability to assign DOIs:
  - [BioStudies](https://www.ebi.ac.uk/biostudies/)
  - [Zenodo](https://en.wikipedia.org/wiki/Zenodo)
  - [Figshare](https://en.wikipedia.org/wiki/Figshare)
  - [Crossref](https://en.wikipedia.org/wiki/Crossref)
  - HCA DCP assigns its own DOIs by becoming a member of the International DOI Foundation (IDF)

#### DOI Versioning
Most open access repositories that provide DOI minting services appear to provide support for versioning (i.e. multiple versions of a single citation available e.g. via a drop-down selection), the ability to store accompanying files in the repository (this would be useful for storing versions of manifest and metadata tsv files, etc.), and the ability to store URL references (useful for linking back to a page in the Data Browser).
Examining how both Figshare and Zenodo do DOIs, as the group minting DOIs controls everything after the prefix, it is possible to have a main DOI which points to the most recent version and then versioned URLs which point to specific versions.

#### Data Browser support for DOIs
The Data Browser could contain a reference to the base DOI for each project. Clicking on the DOI link would then redirect the user to the (external) DOI repository where the user could see all the versions of that project and download the associated manifest and metadata.tsv files for a specific version. 

#### DOI Creation/Update Process
Embedding a DOI in the metadata (e.g. biostudies_doi) during ingest would provide per-project citability.
Provided the DOI repository supports versioning the metadata DOI field could be updated automatically by Ingest to a new version whenever a project update is processed.
Using a DOI that links to an external repository that can store a manifest for each version of a project may be the simplest way to provide access to versions of the data for a project.

The creation/update process would perform the following steps:
  - Ingest creates a new DOI (new project) or a new version of an existing DOI (updated project) when the submission is deemed “complete”.
  - Upload the corresponding metadata tsv and manifest file for this version of the project to the DOI repository (need to wait for these to be generated).
  - Upload the matrix output file(s) for this version of the project to the DOI repository.
  - Update a project description to the DOI repository (e.g. what’s in this version?).
  - Create a link in the DOI repository back to the project details page in the Data Browser.

### Acceptance Criteria [optional]

*Acceptance criteria are the conditions that a RFC must satisfy to be accepted by users or other stakeholders.* 

### Unresolved Questions

Must a data citation provide an *immutable* view of a project? Or is acceptable for any of the following to undergo additions, updates or deletions over time?
  - a project's primary data
  - a project's secondary analysis outputs
  - a project's expression matrix outputs, generated by the Matrix Service
  - a project's metadata
  
If an *immutable* view of the cited data is a requirement, is this technically feasible before a full implementation of support for versioned files (i.e. the AUDR RFC)?

For Phase 1 must a data citation provide a DOI or is a "stable URL" sufficient?

For Phase 2 & 3 does the Data Browser need to provide a view of the cited data on which further faceted searches can be performed?

Can a DOI for the DCP resolve to an external website? We will work with the UX team to come up with pros and cons of using an external authority vs setting up the DCP as a DOI assigning entity and then, if that effort doesn't give us a clear answer, we will ask the Oversight Committee for their view.

### Drawbacks and Limitations [optional]

*Why should this RFC **not** be implemented?*

### Prior Art [optional]

*Share references to prior art to deepen community understanding of the RFC, such as learnings, adaptations from earlier designs, or community standards.*

### Alternatives [optional]

*Highlight other possible approaches to delivering the value proposed in this RFC. 
What other designs were explored? What were their advantages? What was the rationale for rejecting alternatives?*
