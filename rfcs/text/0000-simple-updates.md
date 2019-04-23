### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# Simple Metadata Updates


## Summary

This RFC proposes the scope and design to allow simple metadata additions and updates to be collected by wranglers, reflected end-to-end in the DCP, and made available to consumer services such as the Data Browser and Matrix Service.


## Author(s)

[Laura Clarke](laura@ebi.ac.uk)  
[Justin Clark-Casey](justincc@ebi.ac.uk)  
[Mallory Freeberg]([mfreeberg@ebi.ac.uk](mailto:mfreeberg@ebi.ac.uk)  
[Norman Morrison](norman@ebi.ac.uk)

## Shepherd

_Leave this blank. This role is assigned by DCP PM to guide the Author(s) through the RFC process._

_Recommended format for Shepherds:_

```
[Name](mailto:username@example.com)
```  
## Motivation

Collecting data and metadata to submit to the HCA DCP is a time-consuming process which runs in parallel to conducting scientific experiments. This means that all of the data and metadata for a project might not be submitted at the same time. Additionally, as the HCA pushes for data to be submitted as soon as possible, to expedite its availability to the community, there is an increased risk of the project needing updates or new data and metadata being added.

For these reasons, the DCP must support additions, updates, deletions, and retractions of data and metadata in submitted projects including: adding a publication 1 year after a dataset was submitted, updating descriptive fields due to improvements in understanding and ontological modeling of anatomy or cellular identity, fixing metadata errors or relationships between entities, removing unconsented data.

This RFC proposes a simple technical design to support the simplest metadata updates and additions, recognizing that this is the first step towards a complete solution which will require significant engineering effort.


### User Stories

As a data contributor, I would like to add the details of a scientific publication to my submission to the DCP to broaden knowledge of the work.

As a data contributor, I would like to correct an error I have made in my submission to ensure that the correct metadata is presented to consumers of my data.

As a data wrangler, I would like to update organ system metadata to reflect an improved ontology annotation for this particular organ.  

Read more on the general user experience background for this work: [User Experience Problem Space - Top task “Update my project”](https://docs.google.com/document/d/1FP6kJa3f9NkOHTLqmkNYztls7soZLgeGefxdgR81ONE/edit)

### Definitions

***Addition*** - adding a metadata field:value pair to a metadata document where the metadata field existed in the schema version described by the metadata document  
***Update*** - changing the value of an existing metadata field in a metadata document
***Old update process*** - the current update process which involves adding the existing project to an exclusion list and re-ingesting the entire project - with updates - from scratch as a new submission  
***Data wrangler*** - member of the DCP team who interacts with data contributors to collect metadata updates and additions and drives the process of submitting them to the DCP  
***Experimental design representation*** - the connected set of entities which the DCP uses to represent the experimental design of a project  
***Bundles*** - The data stores a representation of logical sets of data and metadata. There are input and analysis bundles. Input bundles should represent a single set of experimental data such as a lane or plex of sequencing and are defined by ingest as a biomaterial to file transition, connected by an assay process and all entities upwards in the experimental design from that point. Analysis bundles are all the analysis results from a single run of an analysis pipeline on a single input bundle and are defined by ingest as a file to file transition as defined by an analysis process and all entities upwards in the experimental design from that point.


## Scientific "guardrails"

The design proposed in this RFC puts updates triggering an analysis pipeline run out of scope we need some guardrails which ensure we don’t create duplicate analysis results which will confuse our consumers.

There will be both technical and documentation/process driven guardrails.

The first is that the pipeline execution service will check if a pipeline has already been run for a given input data bundle. If it has, the pipeline execution will stop. If it has not, the pipeline execution will continue as though it was a brand new input data bundle.

The second is documentary, some updates will change the specific pipeline or the parameters which should be fed to the pipeline that would run. This solution puts the ability to capture updated analysis and the logic needed to decide if an analysis should be rerun out of scope. The wranglers who are managing the update process will need a document will need a solid understanding of the metadata fields that the data processing pipelines use when running a pipeline both in the notification query and as parameters for the pipeline execution. This will ensure that they don’t use the proposed solution for updates that would require reanalysis. The data processing pipelines team will provide this documentation.

We will create automated monitoring to check if new analysis outputs get associated with an existing input data bundle that already has associated analysis outputs reduce the risk of inadvertent pipeline runs confusing data consumers.


## Detailed Design

### Ingest

**MISSING DIAGRAM**


These are the steps that a wrangler will follow to perform a primary metadata update.


#### Step 1 - Wrangler finds submission

The wrangler finds the project page in the ingest UI by searching over project names and descriptions. Ingest will provide the search capability.

The project page will list all the submissions for that project (at the moment all our projects have only one submission). They will navigate to this submission page.


#### Step 2 - Wrangler retrieves spreadsheet

The wrangler will retrieve a spreadsheet for that submission by clicking a “Download metadata” button or similar on the submission page. The spreadsheet they receive will be the original spreadsheet they submitted plus a column at the end containing the UUIDs of all entities. This column exists only to allow ingest to identify how the rows in the spreadsheet relate to existing metadata when the updated spreadsheet is submitted. They should not be edited by wranglers.


#### Step 3 - Wrangler edits the spreadsheet with their change

The wrangler edits the spreadsheet. They can change any metadata cell apart from the ones containing the UUIDs that ingest added. They can add and remove entries from tabs that contain array/module data. For this RFC, they must not add or remove whole entities (e.g. biomaterials, protocols) or change the linking between entities. Both these are instances of experimental graph change.

#### Step 4 - Wrangler submits spreadsheet

The wrangler will submit a spreadsheet containing an update through a “Submit update” button or similar on the submission page. For each submission, we will allow only one update to be submitted and processed at a time. For this RFC, the wrangler can only submit an updated spreadsheet that validates against the current schemas.

#### Step 5 - Ingest shows the diff to the wrangler for approval

The wrangler will be shown the difference between their updates and the existing submission in the ingest UI. They will have the option either to approve the update, in which case ingest will start submitting it to the datastore, or cancel it, in which case the update will be removed from ingest and they can return to step 3.

#### Step 6 - Ingest uploads new versions of metadata files and bundles to the datastore.

For this RFC, ingest will calculate the bundle updates and submit these to the datastore. If this proves to be a performance problem we will need to consider that problem in a future RFC.

### Cross-DCP

**MISSING DIAGRAM**


Once the bundles are updated, the datastore will send update notifications to all listeners. The query service, data browser and matrix service all need to deal with these notifications appropriately. The analysis service **MUST NOT **submit new or updated analyses to ingest for assays that have previously had analysis submitted. [Secondary analysis ticket #606](https://github.com/HumanCellAtlas/secondary-analysis/issues/606) requests a t-shirt estimate for this aspect of the design. For simple updates ingest will update secondary bundles with primary data (copy forward) at the same time that it updates the primary bundles.


### Acceptance Criteria

*   The wranglers have documentation which allows them to determine if an update will trigger a new pipeline or the same pipeline with different parameters.
*   The wranglers can access the spreadsheet for a particular submission via the ingest UI. The returned spreadsheet will contain all the relevant information to enable the ingest service to carry out the ingest and will use the schema of the original submission.
*   The wranglers can see what documents in a submission have changed via an ingest UI view.
*   A data contributor can see the updated version reflected in downstream services which use that metadata field such as the data browser and the matrix service.
*   A data consumer will have a consistent view of the most recent metadata associated with raw data and secondary analysis bundles presented in services like the data browser or matrix service.
*   A data consumer will not receive duplicate analysis results due to an update.

### Unresolved Questions

This RFC does not represent a complete solution to the Addition and Update functionality that is needed by the DCP. There are a number of unresolved questions and missing functionality that will be left for future RFCs to address.

*   How is the provenance of data and metadata made clear to consumers?
*   How are data retracted without creating meaningless 404 errors for consumers who had used it? The [0004-dss-deletion-process.md]([https://github.com/HumanCellAtlas/dcp-community/blob/master/rfcs/text/0004-dss-deletion-process.md](https://github.com/HumanCellAtlas/dcp-community/blob/master/rfcs/text/0004-dss-deletion-process.md)) considers some of this challenge.
*   How do we update files as opposed to metadata?
*   How do we make the update process through Ingest more straightforward?
*   What logic is needed to make decisions about rerunning data processing pipelines? Where should that logic live? Can that decision be automated?
*   How do we carry out updates which change our representation of the experimental design and change the number or types of entities which are available as part of a submission?
*   What is the interaction between metadata schema changes and updates?

### Drawbacks and Limitations *

There are many types of updates which are not supported by this solution including:

1. Updating to the experiment design representation, ie adding or removing protocols and biomaterials or identification of incorrect relationships between existing biomaterials, processes and files.
2. Updates which require additional or updated experimental data files
3. Updates which require an analysis pipeline to be run again, either a new pipeline or the same pipeline with different parameters
4. Migrating metadata to a new schema where this is necessary before a metadata update can occur. This is being considered independently of updates.
5. Removing experimental data files from a submission.
6. Redacting metadata (in the sense of completely purging it/hard deleting it from the system)
7. Redacting data (in the sense of completely purging it/hard deleting it from the system)
8. Updating metadata on unsubmitted datasets.

Implementing all of these features will take the DCP time and we feel that the proposed design represents a step forward in the update and additional process and is valuable even if not complete. One important issue this design allows us to address which the old update process does not is it will give us an auditable trail of edits to bundles which can be displayed to users, improving their experience of and trust in the platform.


### Prior Art

The update process has been discussed broadly by the DCP in the drafting process of this RFC and during F2F meetings.

Useful outputs from those discussions include:

<span style="text-decoration:underline;">[[Use cases] \
(https://docs.google.com/document/d/1rI8PCASomdAHznyWQceRJGv4-wtg-P5Rm-rI_uJLjtE/edit#)](https://docs.google.com/document/d/1rI8PCASomdAHznyWQceRJGv4-wtg-P5Rm-rI_uJLjtE/edit#)</span>

[[Pre RFC] \
(https://docs.google.com/document/d/1XeAw05RAJe20Q1XDF7eLWLGF3gCme4-8l-ZfhtDWOLg/edit?ts=5beda8b0)](https://docs.google.com/document/d/1rI8PCASomdAHznyWQceRJGv4-wtg-P5Rm-rI_uJLjtE/edit#)

[[Ingest High-Level Design for Updates](https://docs.google.com/document/d/1XeAw05RAJe20Q1XDF7eLWLGF3gCme4-8l-ZfhtDWOLg/edit?ts=5beda8b0) \
([https://docs.google.com/document/d/1YkIqUgZStWC7mnQWHvefONWbUhAcHPDtV6rXExrWhSw/edit#heading=h.i1f06s89qmeh](https://docs.google.com/document/d/1YkIqUgZStWC7mnQWHvefONWbUhAcHPDtV6rXExrWhSw/edit#heading=h.i1f06s89qmeh))

<span style="text-decoration:underline;">[AUDR breakout session in the Feb 2019 F2F] \
([https://docs.google.com/document/d/1LVRqeGqUjLaprLhojCLwL0PiVuv20f7B7W5S59GlBDM/edit](https://docs.google.com/document/d/1LVRqeGqUjLaprLhojCLwL0PiVuv20f7B7W5S59GlBDM/edit)</span>)

[User Experience Problem Space - Top task “Update my project”] \
([https://docs.google.com/document/d/1FP6kJa3f9NkOHTLqmkNYztls7soZLgeGefxdgR81ONE/edit](https://docs.google.com/document/d/1FP6kJa3f9NkOHTLqmkNYztls7soZLgeGefxdgR81ONE/edit))
