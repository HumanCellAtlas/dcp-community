# Charter Process

Each HCA DCP project must have a charter that specifies its scope, responsibilities, and leadership roles.

The charter process provides a simple and consistent path for new projects to be formed that align with the direction and priorities of the HCA DCP.

## What are the roles in the Charter process?

- **Authors** are DCP community members.

- **Approvers** for charters with governance or process deliverables are the Project Leads and Product Owners from *DCP PM*.

- **Approvers** for charters with software deliverables are the Technical Leads from *DCP Architecture*. 

- **Reviewers** are DCP community members, including both software developers and users.

- **Science Reviewer** is the *DCP Science PM* in coordination with *HCA Science Governance*.

## Proposing Charters
### **Authors**:
  - Fork the HumanCellAtlas dcp-community repository
  - Make a subdirectory named `charters/MyProjectName`
  - Copy `charters/charter-template.md` to `charters/MyProjectName/charter.md`
  - Fill in the charter with attention to detail

- Submit a pull request. Add the "charter-community-review" label

- Add a minimum two week _last call_ for the completion of the Community review to the top-level summary comment in the charter pull request.

    **EXAMPLE** 

    **April 1**: Last call for community review

- Request a Community review of the charter on the HumanCellAtlas **#dcp** slack channel. Include a link to the charter pull request and the last call deadline:

    **EXAMPLE** 
  
     ***@channel**: Call for community review of the Data Store charter - https://github.com/HumanCellAtlas/dcp-community/pull/8 - Last call is April 1.*

### **DCP PM**:

- If there is a *Scientific "guardrails"* section in the charter:
  - Assign the **Science Reviewer** as a reviewer of the pull request
  - Add the "science-review-required" label
  - Add a two week _last call_ for the completion of the Science review to the top-level summary comment in the charter pull request.

    **EXAMPLE** 

    **April 5**: Last call for science review

   **NOTE**: *The Science review occurs in parallel to the Community review.*

## Reviewing Charters
[Reviewing Charters]: #reviewing-charters

### **Approvers**:
- May assign specific reviewers in the charter pull request

### **Reviewers**:
- Feedback is added to the pull request comment thread by community members. 

### **Science Reviewer**:
- When the feedback from the Science review is addressed, replace the "science-review-required" label with "science-review-completed".

### **Authors**: 
- Revise the charter pull request in response to feedback and push new commits

- When all issues are addressed and any required Science reviews are complete:
  - Summarize the review discussion for the **Approvers** in the top-level summary comment in the charter pull request
  - Add a minimum one week _last call_ for the completion of the Oversight review to the top-level summary comment in the charter pull request.

    **EXAMPLE** 

    ~~**April 1**: Last call for community review~~

    **April 22**: Last call for oversight review

  - Replace "charter-community-review" with "charter-oversight-review"

- For charters with software deliverables, request an Oversight review of the charter on the HumanCellAtlas **#tech-architecture** slack channel. Include a link to the charter pull request and the last call deadline

  Add the charter as an agenda item to the next *DCP Architecture* meeting

- For charters with governance or process deliverables, request an Oversight review of the charter on the HumanCellAtlas **#dcp-project-mgmt** slack channel. Include a link to the charter pull request and the last call deadline:

    **EXAMPLE** 
  
     ***@channel**: Call for oversight review of the Data Store charter - https://github.com/HumanCellAtlas/dcp-community/pull/8 - Last call is April 22.*

  Add the charter as an agenda item to the next *DCP PM* meeting

    **NOTE**: *Oversight review is limited to **Approvers**. Further community reviews during this period may be disregarded by the **Author(s)**.*

## Approving Charters

### **Approvers**:

- At the *DCP PM* or *DCP Architecture* meeting, review the summary comment. If there is rough consensus to approve the charter, replace "charter-oversight-review" with "charter-approved"

- Merge the pull request

- Announce the approval of the charter on the HumanCellAtlas #dcp slack channel. Include a link to the merged charter:

  **EXAMPLE**

  ***@channel**: DCP Architecture approved the Data Store charter - https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/DataStore/charter.md*

## Requesting substantive changes

### **Approvers**:

- At the *DCP PM* or *DCP Architecture* meeting, review the summary comment. If substantial changes are requested, then replace "charter-oversight-review" with "charter-community-review".

### **Authors**:

- The **Author(s)** must address the changes before requesting a community review and returning the charter to the [community review](#reviewing-charters) process.

## Rejecting Charters

### **Approvers**:

- At the *DCP PM* or *DCP Architecture* meeting, review the summary comment. If there is rough consensus to reject the charter, replace "charter-oversight-review" with "charter-rejected", and close the pull request with the rationale in the comment.

- Announce the rejection of the charter on the HumanCellAtlas #dcp slack channel. Include a link to the charter pull request:

  **EXAMPLE**

  ***@channel**: DCP Architecture declined the Data Store charter - https://github.com/HumanCellAtlas/dcp-community/pull/8*

## Appealing Charter Decisions

Any DCP community member may appeal all charter decisions by **Approvers** (approval, changes requested, or rejection) by sending a message to the *DCP PM* mailing list (pm-team@data.humancellatlas.org).

The message should demonstrate a clear rationale for the appeal, referencing community discussion as needed to support the position.

*DCP PM* must review the appeal, resolve it in a manner of its own choosing, and respond to the original message on the *DCP PM* mailing list within two weeks.