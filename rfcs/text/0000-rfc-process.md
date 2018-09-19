### DCP PR:

# RFC Process

## Summary

The Human Cell Atlas Data Coordination Platform (HCA DCP) evolved informal, consensus-driven processes to coordinate and document technical and governance decisions.

This early *lightweight* and *egalitarian* approach enabled the DCP to achieve rapid velocity and incorporate a diverse set of innovative design concepts.

As DCP continues to mature and scale, our community can more easily contribute and learn when there is a formal and consistent **Request for Comments (RFC)** process to propose enhancements to software or governance which are then maintained in a central repository.

## Author(s)
*DCP PM*

**Note:** *This proposal is based on ideas from the **HCA DCP Technical Decision-Making** white paper written by Bruce Martin with contributions from Tony Burdett, Laura Clarke, Brian O’Connor, Brian Raymor, and Tim Tickle.*

## Shepherd
[Brian Raymor](brianraymor@chanzuckerberg.com)

## Motivation

There are a number of limitations with our current process which need to be addressed:

- For community members who want to propose enhancements to either technical software or governance, it is often unclear where to start - *for example, which HCA slack channel or project member to approach with a proposal.*

  - There is no common template for design proposal submissions.

  - There is no common process describing how the community reviews and approves submitted proposals.

  - There is no formal assistance to guide a contributor through the process. 

- New community members experience difficulties when _onboarding_. There is no curated repository of up-to-date design documents to assist with learning about the software. 

- To ensure consistency of the architectural design, senior technical leaders must monitor multiple work streams or hope that they are notified of potential technical changes - *this is a significant concern for cross-cutting issues such as security or the data model*.

To address these needs, the DCP RFC process defines a transparent and standard method for community members to propose and build consensus around substantial enhancements to DCP software or governance.

### When is an RFC required?

Any proposal that would benefit from additional review or design before being implemented is a good candidate for an RFC.

- An RFC is required if the implemented design would be described in written communication to the community.
- An RFC is required if a design impacts the scope of multiple projects.
- An RFC is required if a technical initiative (refactoring, major architectural change) impacts the community.
- An RFC is required for all changes to DCP governance.  

### Revising RFCs

How are RFCs maintained as *living documents* in an iterative development process where the original design may substantially change based on implementation experiences?

Where necessary, existing RFCs can be revised using the same process (a proposed change submitted as a RFC pull request and subject to community review and approval). **Major changes should result in a new RFC.**

### What are the roles in the RFC process?

- **Authors** are DCP community members.

- **Shepherds** are members of *DCP PM* assigned to assist the **Author(s)** and ensure that the RFC progresses to closure.

- **Approvers** for governance RFCs are the Project Leads and Product Owners from *DCP PM*.

- **Approvers** for software RFCs are the Technical Leads from *DCP Architecture*. 

- **Reviewers** are DCP community members.

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
      - Add the "rfc-proposed" and the appropriate software project name _(such as "Data Store")_ labels. When the RFC crosses the scope of software projects, then the "Architecture" label **MUST** be the project name. *If uncertain about the appropriate project name, then **_Ask a PM_** on the HCA **#dcp-project-mgmt** slack channel*

      For governance RFCs:
        
      - Add the "rfc-proposed" and the "Governance" labels. 

  - Share a link to the RFC pull request for community review on the HumanCellAtlas **#dcp** slack channel

#### DCP PM:
- Assign a **Shepherd** by completing the *Shepherd* section in the RFC template and pushing the update. The **Shepherd** guides the RFC and its **Author(s)** through the process.
- If there is a *Scientific "guardrails"* section in the RFC, assign the **Science Reviewer** as a reviewer of the pull request and add the "science-review-required" label.

#### Shepherd:
- Include the RFC submission in *This week in DCP* status update

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

#### Shepherd:
- Monitor the community review and ensure that issues are addressed by the **Author(s)** in a reasonable time period.

- When all issues are addressed and any required Science reviews are complete, summarize the review discussion for the **Approvers** in a top-level summary comment in the RFC pull request. Replace "rfc-proposed" with "rfc-final-review".

- For software RFCs, share a link to the RFC pull request for approval on the HumanCellAtlas **#tech-architecture** slack channel. Add the RFC as an agenda item to the next *DCP Architecture* meeting.

- For governance RFCs, share a link to the RFC pull request for approval on the HumanCellAtlas **#dcp-project-mgmt** slack channel. Add the RFC as an agenda item to the next *DCP PM* meeting.

- Include the RFC final review in *This week in DCP* status update

### Approving RFCs

#### Approvers:

- At the *DCP PM* or *DCP Architecture* meeting, review the **Shepherd** summary. If there is rough consensus to approve the RFC, replace "rfc-final-review" with "rfc-approved"

#### Author(s):
- Rename the RFC from `0000-my-feature.md` to `rfc####-my-feature.md` (with leading zeros) where `####` is the next available RFC number

- Create a link between the approved RFC and its pull request by updating the *DCP PR* section in the RFC template and pushing the update.

- Merge the RFC pull request

**NOTE**: *The existence of an approved software RFC does not indicate any particular priority or commitment to implement the RFC, nor is an approved RFC a green light to implement. Approved RFCs simply demonstrate community agreement on a technical decision. Product priorities are managed and implementations scheduled via the DCP PM planning process.*

#### Shepherd:

- Validate that the RFC pull request was successfully updated and merged
- Include the RFC approval in *This week in DCP* status update

### Rejecting RFCs

#### Approvers:

- At the *DCP PM* or *DCP Architecture* meeting, review the **Shepherd** summary. If there is rough consensus to reject the RFC, replace "rfc-final-review" with "rfc-declined", and close the pull request with the rationale in the comment.

#### Shepherd:
- Include the RFC rejection in *This week in DCP* status update

#### Author:
- If an RFC is rejected by *DCP Architecture*, then the decision may be escalated for review by *DCP PM* by sending a request to the HumanCellAtlas **#dcp-project-mgmt** slack channel

### Unresolved questions

*What aspects of the design do you expect to clarify further through the RFC review process?*

There is a [pending issue](https://github.com/HumanCellAtlas/dcp-community/issues/25) to define how DCP might document when an approved RFC is implemented.

### Drawbacks / Limitations

The dependence on Git and GitHub in the RFC process may be a barrier to potential contributors, but this is mitigated by the availability of the **Shepherd**. 

## Prior Art

The DCP RFC process is _inspired_ by and _adapted_ from [Rust](https://github.com/rust-lang/rfcs)
and [Kubernetes](https://kubernetes.io/docs/community/keps/).

The Internet Engineering Task Force assigns a [**Shepherd**](https://tools.ietf.org/html/rfc4858#section-3.1) during the _endgame_ of their standards process. 