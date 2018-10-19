### DCP PR:

# RFC Process

## Summary

The Human Cell Atlas Data Coordination Platform (HCA DCP) evolved informal processes to coordinate and document technical and governance decisions.

This early *lightweight* approach enabled the DCP to achieve rapid velocity and incorporate a diverse set of innovative design concepts.

As DCP continues to mature and scale, our community can more easily contribute and learn when there is a formal and consensus-driven **Request for Comments (RFC)** process to propose enhancements to software or governance which are then maintained in a central repository.

## Author(s)
*DCP PM*

**NOTE:** *This proposal is based on ideas from the **HCA DCP Technical Decision-Making** white paper written by Bruce Martin with contributions from Tony Burdett, Laura Clarke, Brian O’Connor, Brian Raymor, and Tim Tickle.*

## Shepherd
[Brian Raymor](brianraymor@chanzuckerberg.com)

## Motivation

There are a number of limitations with our current process which need to be addressed:

- For community members who want to propose enhancements to either technical software or governance, it is often unclear where to start - *for example, which HCA slack channel or project member to approach with a proposal.*

  - There is no common template for design proposal submissions.

  - There is no common process describing how the community reviews and approves submitted proposals.

  - There is no formal assistance to guide a contributor through the process. 

- New community members experience difficulties when _onboarding_. There is no curated repository of up-to-date design documents to assist with learning about the software. 

- Members cannot easily follow the decision making process for major features.

  To ensure consistency of the architectural design, senior technical leaders must monitor multiple work streams which is time consuming, or hope that they are notified of potential technical changes - *this is a significant concern for cross-cutting issues such as security or the data model*.

To address these needs, the DCP RFC process defines a transparent and standard method for community members to propose and build consensus around substantial enhancements to DCP software or governance.

### When is an RFC required?

- An RFC is required for **substantial** additions, deletions, or changes to system behavior or semantics (API, major features, data model, protocols,​ ​service guarantees, architecture). In [semver](https://semver.org/) terms, *major* changes require an RFC, while *minor* changes are often candidates (new API addition).

    **NOTE**: *Internal-only implementation decisions, bug fixes, refactoring, and performance optimization are not substantial changes and can continue to be documented and tracked using github issues.*

- An RFC is required if a design impacts multiple DCP projects.

- An RFC is required for all changes to the DCP formal governance, including but not limited to oversight, decision-making, conflict resolution, and community processes such as charters and RFCs. For more background on governance, see the **Model C: Delegated Governance** section in [Organization & Structure of Open Source Software Development Initiatives](https://dash.harvard.edu/bitstream/handle/1/30805146/2017-03-24_governance.pdf).  

### Revising RFCs

Where necessary, existing RFCs can be revised using the same process (a proposed change submitted as a RFC pull request and subject to community review and approval). **Major changes should result in a new RFC.**

### What are the roles in the RFC process?

- **Authors** are DCP community members.

- **Shepherds** are members of *DCP PM* assigned to assist the **Author(s)** and ensure that the RFC progresses to closure.

- **Approvers** for governance RFCs are the Project Leads and Product Owners from *DCP PM*.

- **Approvers** for software RFCs are the Technical Leads from *DCP Architecture*. 

- **Reviewers** are DCP community members, including both software developers and users.

- **Science Reviewer** is the *DCP Science PM* in coordination with *HCA Science Governance*.

## Detailed Design

### Proposing RFCs

#### **Authors**:

*Before proposing, always engage the DCP community to build consensus and assess whether your proposal sparks interest and addresses a need. You may even discover potential collaborators.*

  - Fork the HumanCellAtlas dcp-community repository
  - Copy `rfcs/rfc-template.md` to `rfcs/text/0000-my-feature.md`
  *- where "my-feature" is descriptive. Don't assign an RFC number yet.*
  
  - Fill in the RFC with attention to detail

  - Submit a pull request.
  
      For software RFCs:
      - Add the "rfc-community-review" and the appropriate software project name _(such as "Data Store")_ labels. When the RFC impacts multiple DCP software projects, then the "Architecture" label **MUST** be the project name. *If uncertain about the appropriate project name, then **_Ask a PM_** on the HCA **#dcp-project-mgmt** slack channel*

      For governance RFCs:
        
      - Add the "rfc-community-review" and the "Governance" labels. 

- Add a minimum two week _last call_ for the completion of the Community review to the top-level summary comment in the RFC pull request.

    **EXAMPLE** 

    **April 1**: Last call for community review

- Request a Community review of the RFC on the HumanCellAtlas **#dcp** slack channel. Include a link to the RFC pull request and the last call deadline:

    **EXAMPLE** 
  
     ***@channel**: Call for community review of the RFC process - https://github.com/HumanCellAtlas/dcp-community/pull/27 - Last call is April 1.*

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
- May assign specific reviewers in the RFC pull request

#### Reviewers:
- Add feedback to the RFC pull request comment thread

#### Science Reviewer:
- When the feedback from the Science review is addressed, replace the "science-review-required" label with "science-review-completed".

#### Author: 
- Revise the RFC pull request in response to feedback and push new commits

### Shepherding RFCs
[Shepherding RFCs]: #shepherding-rfcs
#### Shepherd:
- Monitor the community review and ensure that issues are addressed by the **Author(s)** in a reasonable time period.

- When all issues are addressed and any required Science reviews are complete:
  - Summarize the review discussion for the **Approvers** in the top-level summary comment in the RFC pull request
  - Add a minimum one week _last call_ for the completion of the Oversight review to the top-level summary comment in the RFC pull request.

    **EXAMPLE** 

    ~~**April 1**: Last call for community review~~

    **April 22**: Last call for oversight review

  - Replace "rfc-community-review" with "rfc-oversight-review"

  **NOTE**: *Oversight review is limited to **Approvers**. Further community reviews during this period may be disregarded by the **Author(s)**.*

- For software RFCs, request an Oversight review of the RFC on the HumanCellAtlas **#tech-architecture** slack channel. Include a link to the RFC pull request and the last call deadline

  Add the RFC as an agenda item to the next *DCP Architecture* meeting

- For governance RFCs, request an Oversight review of the RFC on the HumanCellAtlas **#dcp-project-mgmt** slack channel. Include a link to the RFC pull request and the last call deadline:

    **EXAMPLE** 
  
     ***@channel**: Call for oversight review of the RFC process - https://github.com/HumanCellAtlas/dcp-community/pull/27 - Last call is April 22.*

  Add the RFC as an agenda item to the next *DCP PM* meeting

### Approving RFCs

#### Approvers:

- At the *DCP PM* or *DCP Architecture* meeting, review the **Shepherd** summary. If there is rough consensus to approve the RFC, replace "rfc-oversight-review" with "rfc-approved"

#### Author(s):
- Rename the RFC from `0000-my-feature.md` to `rfc####-my-feature.md` (with leading zeros) where `####` is the next available RFC number

- Create a link between the approved RFC and its pull request by updating the *DCP PR* section in the RFC template and pushing the commit.

- Merge the RFC pull request

**NOTE**: *The existence of an approved software RFC does not indicate any particular priority or commitment to implement the RFC, nor is an approved RFC a green light to implement. Approved RFCs simply demonstrate community agreement on a technical decision. Product priorities are managed and implementations scheduled via the DCP PM planning process.*

#### Shepherd:

- Validate that the RFC pull request was successfully updated and merged

### Requesting substantive changes

#### Approvers:

- At the *DCP PM* or *DCP Architecture* meeting, review the **Shepherd** summary. If substantial changes are requested, then replace "rfc-oversight-review" with "rfc-community-review" and return the RFC to the [community review](#shepherding-rfcs) process.

### Rejecting RFCs

#### Approvers:

- At the *DCP PM* or *DCP Architecture* meeting, review the **Shepherd** summary. If there is rough consensus to reject the RFC, replace "rfc-oversight-review" with "rfc-declined", and close the pull request with the rationale in the comment.

### Appealing RFC Decisions

Any DCP community member may appeal an RFC decision (approval or rejection) by sending a message to the *DCP PM* mailing list (pm-team@data.humancellatlas.org).

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

The DCP RFC process is _inspired_ by and _adapted_ from [Rust](https://github.com/rust-lang/rfcs)
and [Kubernetes](https://kubernetes.io/docs/community/keps/).

The Internet Engineering Task Force assigns a [**Shepherd**](https://tools.ietf.org/html/rfc4858#section-3.1) during the _endgame_ of their standards process. 