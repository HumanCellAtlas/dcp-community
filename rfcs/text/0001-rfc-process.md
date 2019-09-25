### DCP PR:

[dcp-community/rfc1](https://github.com/HumanCellAtlas/dcp-community/pull/27)

# RFC Process

## Summary

The Human Cell Atlas Data Coordination Platform (HCA DCP) evolved informal processes to coordinate and document technical and governance decisions.

This early *lightweight* approach enabled the DCP to achieve rapid velocity and incorporate a diverse set of innovative design concepts.

As DCP continues to mature and scale, our community can more easily contribute and learn when there is a formal and consensus-driven **Request for Comments (RFC)** process to propose enhancements to software or governance which are then maintained in a central repository.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED" "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14, [RFC2119](https://www.rfc-editor.org/rfc/rfc2119.txt), and [RFC8174](https://www.rfc-editor.org/rfc/rfc8174.txt) when, and only when, they appear in all capitals, as shown here.

## Author(s)
*DCP PM*

[Arathi Mani](mailto:arathi.mani@chanzuckerberg.com)
 
[Dave Rogers](mailto:dave@clevercanary.com)

**NOTE:** *This proposal is based on ideas from the **HCA DCP Technical Decision-Making** white paper written by Bruce Martin with contributions from Tony Burdett, Laura Clarke, Brian O’Connor, Brian Raymor, and Tim Tickle.*

## Shepherd
[Brian Raymor](brianraymor@chanzuckerberg.com)

## Motivation

There are a number of limitations with our current process which need to be addressed:

- For community members who want to propose enhancements to either technical software or governance, it is often unclear where to start - *for example, which HCA Slack channel or project member to approach with a proposal.*

  - There is no common template for design proposal submissions.

  - There is no common process describing how the community reviews and approves submitted proposals.

  - There is no formal assistance to guide a contributor through the process. 

- New community members experience difficulties when _onboarding_. There is no curated repository of up-to-date design documents to assist with learning about the software. 

- Members cannot easily follow the decision making process for major features.

  To ensure consistency of the architectural design, senior technical leaders must monitor multiple work streams which is time consuming, or hope that they are notified of potential technical changes - *this is a significant concern for cross-cutting issues such as security or the data model*.

To address these needs, the DCP RFC process defines a transparent and standard method for community members to propose and build consensus around substantial enhancements to DCP software or governance.

### When is an RFC required?

- An RFC is REQUIRED for **substantial** additions, deletions, or changes to system behavior or semantics (API, major features, data model, protocols,​ ​service guarantees, architecture). In [semver](https://semver.org/) terms, *major* changes require an RFC, while *minor* changes are often candidates (new API addition).

    **NOTE**: *Internal-only implementation decisions, bug fixes, refactoring, and performance optimization are not substantial changes and can continue to be documented and tracked using github issues.*

- An RFC is REQUIRED if a design impacts multiple DCP projects.

- An RFC is REQUIRED for all changes to the DCP formal governance, including but not limited to oversight, decision-making, conflict resolution, and community processes such as charters and RFCs. For more background on governance, see the **Model C: Delegated Governance** section in [Organization & Structure of Open Source Software Development Initiatives](https://dash.harvard.edu/bitstream/handle/1/30805146/2017-03-24_governance.pdf).  

### Revising RFCs

Where necessary, existing RFCs MAY be revised using the same process (a proposed change submitted as a RFC pull request and subject to community review and approval). **Major changes SHOULD result in a new RFC.**

Minor and trivial changes MAY be made freely, with no announcement by the author(s) of the RFCs. Such changes include correcting typos, re-wordings, and the like.

#### Deprecated RFCs 

When a design is no longer relevant to or implemented in the DCP, the closed PR SHOULD be labeled with "rfc-deprecated." The **Summary** section of the RFC SHOULD be updated to reflect the deprecated status.

### What are the roles in the RFC process?

- **DCP Community Members** are the union of all members of the technical team of the DCP, all members of the DCP PM team and all DCP Users.

- **Authors** are DCP community members.

- **Shepherds** are members of *DCP PM* assigned to assist the **Author(s)** and ensure that the RFC progresses to closure.

- **Approvers** for governance RFCs are the Project Leads and Product Owners from *DCP PM*.

- **Approvers** for software RFCs are the Technical Leads from *DCP Architecture*. 

- **Reviewers** are DCP community members, including both software developers and users.

- **Science Reviewer** is the *DCP Science PM* in coordination with *HCA Science Governance*.

## Detailed Design

### Drafting RFCs

#### **Authors**:

There are three types of RFCs that may be proposed: ***Software***, ***Governance***, and ***Informational***. Software RFCs are expected to be linked to an Objective that is prioritized in the DCP Product Roadmap, tracked by linking directly to an existing ticket in the DCP Zenhub board (usually labeled as a Spike). Informational and Governance RFCs need not directly reference an open ticket in the DCP Zenhub board.

To inform the DCP Community about the development of an RFC, with the intention of forming collaborations between people thinking about similar problems, it is RECOMMENDED that a draft of the RFC be published as soon as the Problem Statement and Authors have been established. The steps to begin are as follows:

   1. Fork the HumanCellAtlas `dcp-community` repository
   2. Copy `rfcs/rfc-template.md` to `rfcs/text/0000-my-feature.md`. Make `my-feature` descriptive but do not assign an RFC number yet.
   3. Fill out the Problem Statement and Author(s) sections.
   4. Push the changes to your branch and create a _Draft_ PR.
   5. Label the PR with the "rfc-draft" label.

#### **Non-Authors**:

When a _Draft_ RFC PR is published and labeled with "rfc-draft," specific commentary is NOT RECOMMENDED (negative or positive) until the Authors(s) propose the RFC for community review. If the draft RFC sparks interest, it is RECOMMENDED that one reach out to the Author(s) offline to suggest a collaboration.

### Proposing RFCs

#### **Authors**:

  - Fill in the RFC with attention to detail
  
  - Submit a pull request. If the PR was originally in a draft state, remove the "rfc-draft" label.
  
  - Add the "rfc-community-review" label.
  
  - Find the auto-generated ticket in the Zenhub `dcp-community` board and assign all Author(s) to the ticket.
  
  - Add a minimum two week _last call_ for the completion of the Community review to the top-level Summary section in the RFC pull request.

    **EXAMPLE** 

    **April 1**: Last call for community review

  - Request a Community review of the RFC on the HumanCellAtlas **#dcp** Slack channel. Include a link to the RFC pull request and the last call deadline:

    **EXAMPLE** 
  
    ***@channel**: Call for community review of the RFC process - https://github.com/HumanCellAtlas/dcp-community/pull/27 - Last call is April 1.*
  
  - For **Software RFCs**:
    
    - Add the appropriate software component name label to the RFC OR if the RFC impacts multiple DCP software components, add the "Architecture" label instead. If uncertain about the appropriate project name, then ask on the **#dcp-project-mgmt** Slack channel.
    
    - Link the ticket that was auto-generated in the `dcp-community` Zenhub board to an open ticket in the `dcp` repo. RFCs generally fulfill a deliverable for a Spike ticket and linking the open ticket in the `dcp` repo to the auto-generated PR ticket guarantees a paper-trail of the work.
    
    - Send an FYI to the HumanCellAtlas **#tech-architecture** Slack channel. Include a link to the RFC pull request and the last call deadline.
  
  - For **Governance RFCs**:
  
    - Add the "Governance" label to the PR.
    
    - Send an FYI to the HumanCellAtlas **#dcp-project-mgmt** Slack channel. Include a link to the RFC pull request and the last call deadline.

#### DCP PM:
  - Assign a **Shepherd** by completing the *Shepherd* section in the RFC template and pushing the commit. The **Shepherd** guides the RFC and its **Author(s)** through the process.

  - If there is a *Scientific "guardrails"* section in the RFC:
  
    - Assign the **Science Reviewer** as a reviewer of the pull request

    - Add the "science-review-required" label

    - Add a two week _last call_ for the completion of the Science review to the top-level summary comment in the RFC pull request.

    **EXAMPLE** 

    **April 5**: Last call for science review

**NOTE**: *The Science review occurs in parallel to the Community review.* 

### Reviewing RFCs

#### Approvers:
- MAY assign specific reviewers in the RFC pull request.

#### Reviewers:
- Add feedback to the RFC pull request comment thread.

#### Science Reviewer:
- When the feedback from the Science review is addressed, replace the "science-review-required" label with "science-review-completed".

#### Author: 
- Revise the RFC pull request in response to feedback and push new commits.

### Shepherding RFCs
[Shepherding RFCs]: #shepherding-rfcs
#### Shepherd:
- Monitor the community review and ensure that issues are addressed by the **Author(s)** in a reasonable time period.

- For Software RFCs, add the RFC as an agenda item to the next *DCP Architecture* meeting.

- For Governance RFCs, add the RFC as an agenda item to the next *DCP PM* meeting.

- When all issues are addressed and any required Science reviews are complete:

  - Summarize the review discussion for the **Approvers** in the top-level summary comment in the RFC pull request.

### Approving RFCs

#### Approvers:

- At the *DCP PM* or *DCP Architecture* meeting, review the **Shepherd** summary. If there is rough consensus to approve the RFC, replace "rfc-community-review" with "rfc-approved"

#### Author(s):
- Rename the RFC from `0000-my-feature.md` to `####-my-feature.md` (with leading zeros) where `####` is the next available RFC number

- Create a link between the approved RFC and its pull request by updating the *DCP PR* section in the RFC template and pushing the commit.

- Merge the RFC pull request

**NOTE**: *The existence of an approved software RFC does not indicate any particular priority or commitment to implement the RFC, nor is an approved RFC a green light to implement. Approved RFCs simply demonstrate community agreement on a technical decision. Product priorities are managed and implementations scheduled via the DCP PM planning process.*

#### Shepherd:

- Validate that the RFC pull request was successfully updated and merged

- Announce the approval of the RFC on the HumanCellAtlas **#dcp** Slack channel. Include a link to the merged RFC:

  **EXAMPLE**

  ***@channel**: DCP PM approved the RFC for the RFC Process - https://github.com/HumanCellAtlas/dcp-community/blob/master/rfcs/text/0001-rfc-process.md*

### Requesting substantive changes

#### Approvers:

- At the *DCP PM* or *DCP Architecture* meeting, review the **Shepherd** summary. If substantial changes are requested, then replace "rfc-oversight-review" with "rfc-community-review".

#### Authors:

- The **Author(s)** must address the changes before requesting a community review and returning the RFC to the [community review](#shepherding-rfcs) process.

### Rejecting RFCs

#### Approvers:

- At the *DCP PM* or *DCP Architecture* meeting, review the **Shepherd** summary. If there is rough consensus to reject the RFC, replace "rfc-oversight-review" with "rfc-declined", and close the pull request with the rationale in the comment.

#### Shepherd:

- Announce the rejection of the RFC on the HumanCellAtlas **#dcp** Slack channel. Include a link to the RFC pull request:

  **EXAMPLE**

  ***@channel**: DCP PM declined the RFC for the RFC Process - https://github.com/HumanCellAtlas/dcp-community/pull/27*

### Pausing RFCs

#### Author(s):

- If via various discussions, it is decided that the RFC must be "paused" until further information can be obtained, and rough consensus has been met to pause rather than reject or withdraw, replace "rfc-community-review" with "rfc-paused" and comment in the PR with the rationale.

#### Shepherd:

- Announce the pausing of the RFC on the HumanCellAtlas **#dcp** Slack channel. Include a link to the RFC pull request:

  **EXAMPLE**

  ***@channel**: DCP PM has decided to pause the RFC in the RFC Process - https://github.com/HumanCellAtlas/dcp-community/pull/27*
  
### Withdrawing RFCs

What is the difference between a withdrawn and a rejected RFC? A rejected RFC has successfully completed review, but is subsequently rejected during its approval stage. A withdrawn RFC is withdrawn from review by its Author(s) prior to the approval stage.

#### Author(s):

- Replace "rfc-community-review" with "rfc-withdrawn" and close the pull request with the rationale in the comment.

#### Shepherd:

- Announce the withdrawal of the RFC on the HumanCellAtlas **#dcp** Slack channel. Include a link to the RFC pull request:

  **EXAMPLE**

  ***@channel**: Authors have withdrawn the RFC for the RFC Process - https://github.com/HumanCellAtlas/dcp-community/pull/27*

### Deprecating RFCs

Over time, it is expected that RFCs that are approved and implemented will eventually be replaced by newer processes or technologies. In this case, an RFC will be deprecated (note that the difference between Deprecated and Withdrawn is that in the former, the RFC has already been approved and implemented while in the latter, the RFC has not yet been approved).

#### Author(s):

- If at any point in time an RFC is no longer implemented within the DCP (either Governance or Technical), the author SHOULD add the label "rfc-deprecated" to the original RFC.
- In addition, it is RECOMMENDED that a short paragraph or note be added in the Summary section of the RFC indicating the deprecated nature of RFC contents with a short explanation of why.
- No announcement is needed and the change should be considered as "trivial" and therefore can be made without going through any formal review process.

### Appealing RFC Decisions

Any DCP community member MAY appeal all RFC decisions by **Approvers** (approval, changes requested, or rejection) by sending a message to the *DCP PM* mailing list (pm-team@data.humancellatlas.org).

The message should demonstrate a clear rationale for the appeal, referencing community discussion as needed to support the position.

*DCP PM* must review the appeal, resolve it in a manner of its own choosing, and respond to the original message on the *DCP PM* mailing list within two weeks.

### Unresolved questions

*What aspects of the design do you expect to clarify later during iterative development of this RFC?*

- [Adding Informational RFCs](https://github.com/HumanCellAtlas/dcp-community/issues/30)
- [Tracking the implementation state of approved RFCs](https://github.com/HumanCellAtlas/dcp-community/issues/25)
- [Improving community notifications for RFCs](https://github.com/HumanCellAtlas/dcp-community/issues/37)

### Drawbacks / Limitations

The dependence on Git and GitHub in the RFC process may be a barrier to potential contributors, but this is mitigated by the availability of the **Shepherd**. 

## Prior Art

The DCP RFC process is _inspired_ by and _adapted_ from [Rust](https://github.com/rust-lang/rfcs) and [Kubernetes](https://kubernetes.io/docs/community/keps/).

The Internet Engineering Task Force assigns a [**Shepherd**](https://tools.ietf.org/html/rfc4858#section-3.1) during the _endgame_ of their standards process.