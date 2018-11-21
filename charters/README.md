# Charter Guide

Each HCA DCP project must have a charter that specifies its scope, responsibilities, and leadership roles.

The charter process provides a simple and consistent path for new projects to be formed that align with the direction and priorities of the HCA DCP.

### What are the roles in the Charter process?

- **Authors** are DCP community members.

- **Approvers** for charters with governance or process deliverables are the Project Leads and Product Owners from *DCP PM*.

- **Approvers** for charters with software deliverables are the Technical Leads from *DCP Architecture*. 

- **Reviewers** are DCP community members, including both software developers and users.

- **Science Reviewer** is the *DCP Science PM* in coordination with *HCA Science Governance*.


## What the process is
  - Fork the HumanCellAtlas dcp-community repository
  - Make a subdirectory named charters/`MYPROJECTNAME`
  - Copy `charters/charter-template.md` to `charters/MYPROJECTNAME/charter.md`
  - Fill in the charter with attention to detail
  - Submit a pull request. Add the "charter-community-review" label
  - Share a link to the pull request for community review on the HumanCellAtlas **#dcp** slack channel
  - Feedback is added to the pull request comment thread by community members. The Science PM (or a delegate) reviews for appropriate oversight from HCA Science governance.
  - Update the pull request in response to feedback

  **For charters with software deliverables:**

  - Share a link to the pull request for approval on the HumanCellAtlas **#tech-architecture** slack channel when all feedback is addressed. Replace "charter-community-review" with "charter-oversight-review".

    **NOTE**: *Oversight review is limited to **Approvers**. Further community reviews during this period may be disregarded by the **Author(s)**.*
     
  - The request will be discussed at the next meeting of DCP Architecture (Technical Leads). If approved, the Technical Leads replace "charter-oversight-review" with "charter-approved" and merge the pull request. If rejected, the Technical Leads replace "charter-oversight-review" with "charter-rejected" and add a rationale to the pull request comment thread.

  **For charters with governance or process deliverables:**

  - Share a link to the pull request for approval on the HumanCellAtlas **#dcp-project-mgmt** slack channel when all feedback is addressed. Replace "charter-community-review" with "charter-oversight-review".

    **NOTE**: *Oversight review is limited to **Approvers**. Further community reviews during this period may be disregarded by the **Author(s)**.*

  - The request will be discussed at the next meeting of DCP PM (Project Leads and Product Owners). If approved, the PM(s) replace "charter-oversight-review" with "charter-approved" and merge the pull request. If rejected, the PM(s) replace "charter-oversight-review" with "charter-rejected" and add a rationale to the pull request comment thread.
