### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# HCA Project completion stage RFC

## Summary

This RFC proposes a process of releasing metadata and data from an HCA project only after it has been completed by the DCP in order to maximize the value to the data consumer through reducing user confusion and minimizing errors in the final product.

## Author(s)

 [Jason Hilton](mailto:jahilton@stanford.edu)

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Definitions

**HCA submission**: a set of raw data derived from the same experimental process, and the metadata describing that process

**HCA project**: an HCA submission plus any DCP products derived from that HCA submission

An HCA project is considered to be **version-complete** when the DCP has delivered every possible product, given current DCP capabilities, for a given HCA submission.

## Motivation

**Display only completed data:**
Any HCA project with a corresponding analysis pipeline should be considered in progress until all downstream analysis products are generated, validated, and available. Currently, a submitted HCA project is made accessible to the public as soon as raw data & metadata are submitted. The trickling in of data can lead to data consumer confusion as to if/when more data will be added to the HCA project. The proposed solution allows the DCP to complete the version of the HCA project with DCP outputs before the HCA project becomes publicly accessible.

**More thorough data validation:**
The downstream services of the DCP have the potential to uncover metadata and data errors. Viewing the version-complete HCA project as the user will view it can also identify errors. A solution to this is a setting on HCA projects allowing signed-in DCP members to access the data in order to perform a final ‘sanity check’.

**Support HCA submitters (Phase 2):**
Allowing HCA submitters to view the final product in their collaboration with the DCP before it is released adds yet another validation step (after all, who knows their data better?) and is a valuable step in good faith data sharing. The inclusion of a data lifecycle tracker on the HCA project page keeps the HCA submitter up-to-date on their HCA submission and ensures a transparent process.

### User Stories

As a consumer of HCA data, I would assume that an HCA project is version-complete if it is displayed on the DataBrowser or searchable in the DataStore and will not check back for more data analysis products.

As a consumer of HCA data, I will be more confident in using raw data in my own analysis if they have been successfully run on the DCP-hosted pipelines.

As a consumer of HCA data, I will be more confident in using metadata, raw data, or analysis products from an HCA project if all data and data provenance are available and easily referenced.

As a consumer of HCA data, I do not want to see HCA Integration Test projects on the DataBrowser or DataStore, especially if I have a notification set up for new HCA projects as these tests will trigger my notification but don’t actually represent new data.

As a member of the DCP, I cannot do a comprehensive final check on data on the DataBrowser or DataStore unless I see it as the data consumer will see it.

As a member of the DCP, I do not want inaccurate metadata or products of incorrect data analysis being shown to the public. 
Examples of inaccuracies that were/are publicly accessible:
- Optimus results on Smart-seq2 data ([#2314](https://github.com/HumanCellAtlas/data-store/issues/2314))
  - Impact: Incorrect analysis results are accessible to the user. Resulted in breaking data permanency as the data were removed with no record of provenance.
- ‘Homo sapiens’ vs ‘homo sapiens’ ([ZD ticket 158](https://humancellatlas.zendesk.com/agent/tickets/158))
  - Impact: Inconsistent access experience across HCA projects (a user specifying one value will get results, but not all desired results).
- HCA Project ingested at the wrong schema version ([#2421](https://github.com/HumanCellAtlas/data-store/issues/2421))
  - Impact: Inconsistent access experience across HCA projects. Resulted in breaking data permanency as the data were removed and resubmitted with new uuids and HCA project page url with no record of provenance.
- HCA Project page Metadata tsv download threw an error
  - Impact: A non-functioning user-accessible link. Inconsistent access experience across HCA projects if the metadata tsv is not available for all HCA projects.
- HCA submitter name misspelled ("Tom Mithcell")
  - Impact: A (rather personal) HCA submitter-facing error will put DCP credibility at risk.

As an HCA submitter whose name is associated with the HCA project, I want to be allowed time to review the full details of my HCA submission as for accuracy before it becomes publicly available. I want this review to be through the vantage of a potential user. (Phase 2)

As an HCA submitter, I am curious where my HCA submission is in the DCP’s data lifecycle after I have provided the metadata spreadsheet all the way until it is released. (Phase 2 + Phase 'Dashboard')

## Functional Specifications
**Phase 1**
*MVP requirements*
- A mechanism for DCP members to sign-in to the DataBrowser and DataStore.
- A mechanism for an HCA project to be visible only for a user that is signed-in.
- Ensure Secondary Analysis and Matrix Service are able to access the data hidden from signed-out users and to submit their products back to the HCA project.
- A mechanism for Data Operations to switch an HCA project from not visible for signed-out users to visible for all users (data release).
- A ‘checklist’ and validation procedure for Data Operations to perform on each HCA project within a week of being deemed version-complete.

**Phase 2**
*In anticipation of a logical breakpoint in technical effort required and priority of functionalities, the implementation of DCP-only access (Phase 1) should not be delayed by these additional requirements.*
- A mechanism for an HCA submitter to sign-in to the DataBrowser and DataStore, and be given access their own HCA project before it is visible for all users.
  - This will be restricted to named contributors on the project.
  - This will be incorporated in the Data Operations validation checklist, and thus, restricted to same one-week timeframe.

**Phase 'Automation'**
*This can happen at any time within or after Phase 1*
- Automation of the Data Operations validation procedure.

**Phase 'Dashboard'**
*This can happen at any time within or after Phase 1, it adds little value until Phase 2 is implemented, given the DCP internal tracking*
- A dashboard on the HCA project page for signed-in users to track where a given HCA project is in the data lifecycle.

### Acceptance Criteria

**Phase 1**
- A project's metadata and data can be fully accessible to each individual DCP member via the DataBrowser and DataStore while fully inaccessible to users outside of the DCP.
- A project's metadata and data are fully accessible to Secondary Analysis and Matrix Service while fully inaccessible to a user outside of the DCP.
- A Data Operation member is able to switch an HCA project from inaccessible to accessible for users outside of the DCP.
- Data Operations has published a QA checklist and validation procedure and carries out the procedure for each HCA project within a week of being deemed version-complete.

**Phase 2**
- A project's metadata and data can be fully accessible to each named contributor on the project, as well as each individual DCP member, via the DataBrowser and DataStore while fully inaccessible to users outside of the DCP.

**Phase 'Automation'**
- 75% of the Data Operations validation procedure is automated.

**Phase 'Dashboard'**
- A project that is inaccessible to users outside of the DCP displays a dashboard with the current status of the project in the data lifecycle, viewable by a DCP member (and, after Phase 2, a named contributor on the project).

### Unresolved Questions

**Aspects of the design to discuss and clarify later during iterative development of this RFC**
- How to distinguish 'unreleased' from 'released' projects
- A mechanism to gain access to 'unreleased' projects on the DataBrowser
- A mechanism to gain access to 'unreleased' projects in the DataStore

**Future considerations**: There will have cases in the future where a project will have more raw data and/or analysis data and/or matrix products to a project after it has been released. The new data should bee inaccessible to the public until version-complete and validated, while all of the released metadata and data should remain released. This will likely trigger a new project-version (although a concepet not yet realized). Therefore, the unreleased/released distinction should be at the _prroject-version_ level. Another option would be to put the unreleased/released distinction at the file level.

### Drawbacks and Limitations

This postpones the delivery of experimental metadata, raw data, and Secondary Analysis products as a project continues through the DCP data lifecycle. It is believed that data consumers will accept this minimal delay because it provides them with more confidence in their data access (they encounter fewer errors and there is no uncertainty as to whether more products will be delivered in the near future for a given project). 
