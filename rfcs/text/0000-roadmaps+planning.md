### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`


# Roadmaps and Planning for the DCP

## Summary

This RFC documents the Human Cell Atlas (HCA) Data Coordination Platform (DCP) best practices for product roadmaps and project management to deliver product increments of the DCP "data as a service" on a quarterly cadence.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED" "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14, [RFC2119](https://www.rfc-editor.org/rfc/rfc2119.txt), and [RFC8174](https://www.rfc-editor.org/rfc/rfc8174.txt) when, and only when, they appear in all capitals, as shown here.

## Authors

[Cara Mason](mailto:cara@broadinstitute.org)

[Brian Raymor](mailto:brianraymor@chanzuckerberg.com)

## Contributors

[Trevor Heathorn](mailto:theathor@ucsc.edu)

[Matt Weiden](mailto:mweiden@chanzuckerberg.com)

  The original roadmap process was "prototyped" by the DCP Project Leads.

## Shepherd

  [Brian Raymor](mailto:brianraymor@chanzuckerberg.com)

## Motivation

The DCP enables large-scale impact of centralized data generated from the HCA; however, one of the **challenges** facing the DCP as it matures was identified in this [Update from HCA Science Governance](https://docs.google.com/presentation/d/13hZ7vmr371mN1YFjFyDN-6c0Ul95NwiJcVsjA3nJFVE/edit#slide=id.p3):

> "A lack of reproducible planning process that prioritizes deliverables and ensures confidence that they will meet the needs of HCA"

To deliver on the large-scale impact of centralized data, this RFC defines a transparent, iterative process to plan, align, and execute work across the DCP.

To best serve our users and address the concerns of HCA Science Governance, this process focuses on the needs and priorities of DCP "data as a service", rather than the individual components. This requires a DCP-wide project planning and management process with clear priorities and product increments delivered on a regular cadence.

## Definitions

**Users of the DCP** include: Computational and non-computational consumers of the DCP, scientific data contributors to the DCP, and methods and application developers.

**Project Leads**, **Product Owners**, and **User Experience** are members of the PM team as described in its [charter](https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/PM/charter.md). Individual Project Leads and Product Owners are identified in [component charters](https://github.com/HumanCellAtlas/dcp-community/tree/master/charters).

**Technical Leads** are core members of the Architecture Team as described in its  [charter](https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/Architecture/charter.md). Individual Technical Leads are identified in [component charters](https://github.com/HumanCellAtlas/dcp-community/tree/master/charters).

**Themes** represent a discrete user or stakeholder need that delivers on the long term vision of the HCA. A Theme motivates a collection of work performed by the DCP.

**Roadmap Objectives** are the concrete elements of an overarching Theme.

**Product Roadmaps** identify and express user needs modeled as high-level Themes that capture user priorities. Roadmaps align the DCP community around a common set of Themes and get scientists excited about future releases.

**Science Roadmaps** communicate a high-level vision of the goals of the single-cell community and a trajectory of progress towards those goals. It identifies both use cases for the constructed Atlas and open scientific questions that must be solved to build it.

**UX Insights** are summaries from research activities performed by User Experience that document concrete and actionable recommendations to help DCP teams make user-centered decisions during planning and design.

**[User Stories](http://agiledictionary.com/277/user-story/)** are requirements "stated as a sentence or two of plain English. A user story is often expressed from the user’s point of view, and describes a unit of desired functionality."

**[Epics](http://agiledictionary.com/309/epic/)** 
are a collection of User Stories required to complete an objective.

**[Spikes](http://agiledictionary.com/209/spike/)** are adopted from eXtreme Programming for activities that require research or design before estimation and final implementation can occur.
- [Example use in the Scaled Agile Framework](https://www.scaledagileframework.com/spikes/)

**Milestones** represent a four week timeline/duration in a Release.

**Releases** are composed of three milestones and occur on a quarterly cadence.

## DCP User Experience (UX) Activities and Insights

User Experience (UX) Research, Design and Evaluation activities SHOULD be a part of every aspect of the product life cycle, informing the overall DCP strategy, Product Roadmap, Epics and user stories.

It is the responsibility of the respective teams to engage in UX activities throughout the product lifecycle. The UX team MUST either lead or provide training and consultancy for some of these activities.

Insights and recommendations from UX activities SHOULD be shared and acted upon throughout the product life cycle. A summary of the insights from various UX activities SHOULD be delivered at each iteration of the Product Roadmap to inform decisions about future changes or developments.

The UX team MAY also create a wider DCP user survey on an annual basis to collect additional attitudes from the scientific community.

## Science Roadmap

At minimum, the Oversight Committee MUST provide an annual Science Roadmap to Project Leads to be reviewed at the next Oversight Committee meeting following its receipt.

Project Leads MUST clarify outstanding questions about the Science Roadmap with the Oversight Committee within two weeks of its receipt.

## Detailed Design for Product Roadmaps

The Product Roadmap creates a shared understanding across the DCP community and Users of the DCP about our investments and product direction for multiple quarters.

Themes and Objectives from the Product Roadmap are the primary source of priorities for planning Releases each quarter.

### Timeline for Roadmaps

Work MAY be completed earlier at any point during the timelines below but MUST be completed no later than the final day of the timebox. For example, the Technical Objectives may be refreshed during M1W3. 

| Milestone+Week (Current Release) | Deliverable |
|:--:|-|
|M2W1|[Refreshing Technical Objectives](#refreshing-technical-objectives)|
|M2W2|[Refreshing the Product Roadmap](#refreshing-the-product-roadmap)|
|M2W3|[Reviewing the Product Roadmap with HCA Community](#reviewing-the-product-roadmap-with-hca-community)|
|M2W4|[Addressing Comments from HCA Community Review](#addressing-comments-from-hca-community-review)|
|M3W1|[Reviewing the Product Roadmap with the Oversight Committee](#reviewing-the-product-roadmap-with-the-oversight-committee)|
|M3W2|[Approving and Publishing the Product Roadmap](#approving-and-publishing-the-product-roadmap)|
|M3W3|[Clarifying the Next Release](#clarifying-the-next-release)|
|M3W4|... |

### Shepherding the Roadmap Process

Project Leads MUST identify a Roadmap Shepherd (Project Manager). The Shepherd MUST NOT be a Project Lead, to ensure the neutrality of the content for the Product Roadmap.

The Shepherd is responsible for:
* Managing the timeline and facilitating all meetings for the refresh of the Product Roadmap
* Documenting important decisions, discussions, and questions 
* Assigning action items to move toward the publication of the Product Roadmap, should no Project Lead volunteer, and following up with them through completion

### Refreshing Technical Objectives 
[Refreshing Technical Objectives]:#refreshing-technical-objectives

Through a process of their own choosing, the Technical Architecture team MUST refresh critical architectural priorities and technical debt in their *living* Technical Objectives Informational RFC 
as input to each iteration of the Product Roadmap. The RFC SHOULD outline technical strategies and estimate cost (such as two engineers at 20%) to realize its objectives. 

The RFC MUST be approved prior to [Refreshing the Product Roadmap](#refreshing-the-product-roadmap) for consideration.

### Refreshing the Product Roadmap 
[Refreshing the Product Roadmap]:#refreshing-the-product-roadmap

* Prerequisite: Science Roadmap
* Prerequisite: Technical Objectives RFC
* Prerequisite: UX Insights

In preparation, Project Leads MUST independently review the prerequisites and document their proposed _"diffs"_ to the *living* Product Roadmap Informational RFC including but not limited to:

* Selecting and integrating Technical Objectives into the Product Roadmap 
* Adding, Deleting, Revising, Re-prioritizing Themes and Objectives with a documented rationale

Additions or Revisions MUST use the Roadmap Template format. 

Project Leads MUST review these proposed _"diffs"_ as a collective and **decisively** distill into a draft. This is achieved through a consensus building process, which is implemented by the Roadmap Shepherd.

The Roadmap Shepherd MUST then update the Product Roadmap Informational RFC to reflect the revised content from the draft.

When consensus is reached, the Product Roadmap is submitted as a pull request to the *dcp-community* repository.

If the Project Leads fail to reach [rough consensus](https://tools.ietf.org/html/rfc7282) by the specified deadline, then the Roadmap Shepherd MUST:

* Export the markdown version of the incomplete, draft Product Roadmap to Google Docs and and annotate areas of conflict
* Forward this incomplete draft to the Oversight Committee for escalation and resolution

### Reviewing the Product Roadmap with HCA Community
[Reviewing the Product Roadmap with HCA Community]:#reviewing-the-product-roadmap-with-hca-community

The Roadmap Shepherd MUST export the markdown version of the draft Product Roadmap to Google Docs for HCA community review. This community may be unfamiliar with GitHub.

The Roadmap Shepherd MUST announce the draft Product Roadmap to the membership of the Human Cell Atlas (through its the Registration mailing list) with a minimum of a one week last call for responses.

#### Addressing Comments from HCA Community Review
[Addressing Comments from HCA Community Review]:#addressing-comments-from-hca-community-review

The Roadmap Shepherd MUST ensure that the Project Leads (Authors) respond to comments, answer questions, and update the draft Product Roadmap as required.

### Reviewing the Product Roadmap with the Oversight Committee
[Reviewing the Product Roadmap with the Oversight Committee]: #reviewing-the-product-roadmap-with-the-oversight-committee

The Roadmap Shepherd MUST export the markdown version of the draft Product Roadmap to Google Docs and forward to the Oversight Committee for review. This Committee may be unfamiliar with GitHub.

The Project Leads MUST iterate with the Oversight Committee and revise the draft Product Roadmap until it meets with the approval of the Committee.

The Product Roadmap MUST be approved by the Oversight Committee approved prior to [Clarifying the Engineering Plan](#clarifying-the-engineering-plan). 

#### Approving and Publishing the Product Roadmap
[Approving and Publishing the Product Roadmap]: #approving-and-publishing-the-product-roadmap

Once approved, the Roadmap Shepherd MUST merge the update to the Product Roadmap Informational RFC in the *dcp-community* repository and announce its approval on the HumanCellAtlas *dcp* slack channel.

## Detailed Design for Planning and Execution

### ZenHub for Project Management

The [ZenHub](https://www.zenhub.com/) project management application has been partially adopted in the DCP community. This RFC fully commits the community to ZenHub as our *one source of truth* for project management and engineering plans.

All DCP component repositories that will have issues contained in Epics in the _dcp_ repository and assign items to a DCP Release and Milestone MUST be connected to the ZenHub *DCP* Workspace. 

What are [ZenHub Workspaces](https://help.zenhub.com/support/solutions/articles/43000504792-workspaces-overview-what-s-new-)?

> A Workspace is how you bundle GitHub repositories into a single view and can consist of a single or multiple GitHub repositories. With Workspaces we have removed the 1-1 mapping of Board-to-Repo, allowing a repository to be added to multiple Boards. In other words, Issues and Epics can be added and updated in different Workspaces at the same time yet have an independent pipeline allocation for each Workspace.

The [DCP PM Team](https://github.com/orgs/HumanCellAtlas/teams/dcp-pm-team) in the Human Cell Atlas GitHub organization MUST have write access to all the repositories in the *DCP* Workspace to permit the creation of Releases, Milestones, and child issues for Epics.

DCP SHOULD NOT duplicate the ZenHub DCP Board in other documents or spreadsheets. The Board and its reports represent the definitive list of DCP work items for a Release and their current state. 

### DCP Product Backlog

The DCP ZenHub board is the Product Backlog for DCP. All DCP component repositories that will assign items to a DCP Release and Milestone MUST be added to the ZenHub *DCP Backlogs* workspace. The [DCP PM Team](https://github.com/orgs/HumanCellAtlas/teams/dcp-pm-team) in the Human Cell Atlas GitHub organization MUST have write access to all the repositories in the *DCP workspace*.

The top-level *dcp* repository MUST contain **all** significant work that is important to the DCP community and requires visibility and prioritization. This includes Roadmap objectives and product-wide technical requirements (e.g. “components should have logs...“). 

[**High Priority** status](https://help.zenhub.com/support/solutions/articles/43000495285-setting-issues-as-high-priority) MUST only be assigned by DevSecOps to issues in the *dcp* repository deemed critical or high-severity. 

If Institutions are using other project management applications such as Jira for internal requirements and reporting, then all DCP related issues in a Release MUST be *projected* into ZenHub for transparency and finer granularity for tracking progress.

To avoid maintenance overhead due to duplicate or incomplete issues, the creation of new items in the *dcp* repository SHOULD be limited to the PM Team or a facilitator from the Architecture Team.

DCP Product Backlog items are modeled as User Stories, Epics, and Spikes. 
 
#### Owners

A DCP owner MUST be assigned to each issue in the Product Backlog in the *dcp* repository to coordinate progress and drive the issue to on-time closure. This owner is responsible for ensuring that related issues are created in component repositories and linked to Epics within the *dcp* repository.

For example, an implementation task for a specific component resides in its own repository and is linked to the relevant Epic in the *dcp* repository.

#### User Stories

User stories are often in the form of simple templates such as:

`As a <type of user>
I want to <action>
so that <result>`

DCP issues SHOULD include a User Story, Acceptance Criteria ("Definition of Done"), and a Deliverable to be demonstrated during the Release Retrospective to the Oversight Committee.

The `<type of user>` MUST be specific and reference [HCA Personas](https://docs.google.com/presentation/d/1bsu8q9CzRXv3Y8c1p8NENi3p8KlxC4scp2tJkHO3Jeo/edit#slide=id.g3cf4e46e8f_0_38) when appropriate.

*As a data wrangler working on the DCP, I want to adapt the metadata schema quickly in order to respond to developments in experimental approaches and new assays.*

*Acceptance Criteria*
- *[ ] This*
- *[ ] That*
- *[ ] And one more thing*

*Deliverable that can be demonstrated to the Oversight Committee*

...

### Planning and Executing a Release

The DCP community members SHOULD respond to requests related to Release planning and execution in a timely manner. If there is a failure to engage and be responsive, then a gentle escalation to the DCP PM slack channel is RECOMMENDED to ensure that progress occurs and the schedule does not slip. 

#### Timeline for Planning and Execution

Work MAY be completed earlier at any point during the timelines below but MUST be completed no later than the final day of the timebox.

| Milestone+Week (Current Release) | Deliverable |
|:--:|-|
|M3W3|[Clarifying the Next Release](#clarifying-the-next-release)|

---

| Milestone+Week (Next Release) | Deliverable |
|:--:|-|
|M1W1|[Sketching the Release](#sketching-the-release)|
|M1W2|[Creating the Engineering Plan](#creating-the-engineering-plan)|
|M1W3|[Checkpoint for Milestone 1](#checkpoint-for-milestone-1)|
|M2W3|[Checkpoint for Milestone 2](#checkpoint-for-milestone-2)|
|M3W3|[Checkpoint for Milestone 3](#checkpoint-for-milestone-3)|

Release planning begins in the third Week of the third Milestone of the current Release. For example Q4 planning begins in Q3M3W3 and continues into Q4M1. To reiterate, Planning continues into Milestone 1 of a Release as needed.

#### Clarifying the Next Release
[Clarifying the Next Release]:#clarifying-the-next-release

* Prerequisite: Product Roadmap
* Prerequisite: UX Insights

The Roadmap Shepherd schedules a two-hour meeting with DCP PM and the Technical Leads to review any incomplete Objectives in the Release, the next Objectives in the DCP Product Roadmap, and critical UX Insights.

Incomplete Objectives are the highest priority for the next Release unless emerging conditions, as identified by the Project Leads, have altered the priorities for the previous Release plan.

The outcome from this meeting is the proposed set of Product and Technical Objectives for the next Release. One (and only one) Product Owner MUST be assigned to each proposed Objective. This Product Owner is responsible for refining and driving the Objective to on-time closure in coordination with Technical Leads and other collaborators.

The dates for the next ZenHub Release and Milestones MUST be assigned or updated in this meeting.

DCP PM MUST assign a rotating Release Shepherd to help the Product Owners and Technical Leads observe the processes and practices for the Release as outlined in this RFC. 

#### Sketching the Release
[Sketching the Release]:#sketching-the-release

For each Objective in the Release, its Product Owner MUST create one or more Epics in the *dcp* repository that document User Stories, Acceptance Criteria, and the Deliverable to Demonstrate.

If an Objective is *well-understood*, then the Product Owner, in consultation with the relevant Technical Leads, MUST assign either a Milestone or a T-Shirt Size label for rough costing and capacity planning to the issue. Pre-defined T-Shirt Sizes are:

| Size | Duration                |
|:----:|-------------------------|
|  XXS | 1 week                  |
|  XS  | 2 weeks                 |
|   S  | 4 weeks (1 X Milestone) |
|   M  | 8 weeks (2 X Milestone) |
|   L  | 12 weeks (1 X Release)  |
|  XL  | 24 weeks (2 X Release)  |
|  XXL | 36 weeks (3 X Release)  |

Based on rough costing, Technical Leads and their engineering teams SHOULD provide an initial assessment for "fit" - whether any proposed Epics need to be cut or reduced in scope for the Release.

It is strongly RECOMMENDED that engineering teams reserve a **minimum of 30%** capacity during a Release including but not limited to:

* Responsibilities as a Release Engineer or Release Manager when [releasing new versions of software](https://allspark.dev.data.humancellatlas.org/dcp-ops/docs/wikis/SOP:%20Releasing%20new%20Versions%20of%20DCP%20Software)
* Reserving time during Milestone 3 to concentrate on stability and *bug bashing* activities
* Emerging critical security or operational issues 
* Contributions to community reviews for DCP RFC(s)
* Integration between components
* Regular DCP meetings
* Vacations, holidays, and sick days
* Interviewing and staff transitions

This **minimum of 30%** also incorporates guidance from the [pending] **HCA DCP Hotfix Process** that manages *"security vulnerabilities and other high-level risks arise outside of the regular planning cadence"*. It recommends that engineering teams reserve an additional 15% in the Q4 2019 and Q1 2020 Releases. This recommended percentage will be tuned in the future based on metrics collected.

#### Modeling Objectives Requiring Further Research

If an Objective requires further exploration or design before it can be estimated and implemented, then its Product Owner MUST:

1. Create a new ZenHub issue and label it as a Spike.
1. Assign one (and only one) Owner responsible for completing the Spike.
1. Document and monitor the *work-in-progress*. If an RFC is required, a reference to the draft RFC in the *dcp-community* repository is required; otherwise a reference to other documentation is appropriate. In both cases, the reference SHOULD be documented in the top-level summary comment of the Spike for _at a glance_ review.
1. Document its Acceptance Criteria such as an approved RFC. 
1. Add the Spike to its related Epic.
1. Add the Spike as a blocking dependency in its related Epic.
1. Assign the Spike to a specific Milestone in the Release. Spikes have a finite duration. If the Acceptance Criteria for the Spike is an RFC, then the Milestone SHOULD reflect when the RFC is expected to be approved.

For collaborative designs, it is RECOMMENDED that a temporary public working group channel be created in *HumanCellAtlas* Slack and documented in the top-level summary comment of the Spike to ensure that the *state of play* is visible to the DCP community.

The related Epic cannot be estimated and refined until the completion of its blocking Spike. Once the Spike is closed, then its Epic MUST be modeled as outlined in [Creating the Engineering Plan](#creating-the-engineering-plan).


#### Modeling Objectives that Require Multiple Releases

Objectives which span multiple Releases MUST be decomposed into one or more Epics per Release. The Epics MUST be assigned to a Release and linked as blocking dependencies. For example, *Epic 1 in Release 1 blocks Epic 2 in Release 2 blocks Epic 3 in Release 3* which clearly documents their dependency chain and schedule.

An RFC is strongly RECOMMENDED for Objectives that require multiple releases to complete.

#### Creating the Engineering Plan
[Creating the Engineering Plan]:#creating-the-engineering-plan

In consultation with their relevant Technical Leads and collaborators, the Product Owner MUST complete the refinement and modeling for *well-understood* Epics, including the addition of child issues for product readiness and the surfacing of dependencies between software components using ZenHub Dependencies.

Milestones MUST be assigned to all issues assigned to the Release not blocked by a Spike.

It is RECOMMENDED that caution be exercised when scheduling issues in a dependency chain in the same Milestone. Historically, this has been the source of slips in DCP schedules.

The Release Shepherd MUST validate accurate modeling of Objectives for the Release in ZenHub and create a Release epic named “Q#YYYY” such as “Q42019” which contains all the related *dcp* Epics and issues for the Release. This will automatically include all child issues in other component repositories.

This Epic represents the Engineering Plan which MUST be sent to the DCP PM mailing list for review and approval, noting any Objectives that were cut or reduced in scope due to engineering capacity. The Project Leads MUST respond to the original message within two days and MAY negotiate changes to the Engineering Plan. 

#### Changes to the Engineering Plan

During a Release, if an issue that models a Roadmap Objective in the Engineering Plan slips from its original or previously updated Milestone, it MUST be identified and a reason for the change MUST be documented in the top-level summary comment of that issue. 

For issues that have not been assigned a Milestone due to a blocking dependency on a Spike, if the Spike slips from its original or previously updated Milestone, it MUST be identified and a reason for the change MUST be documented in the top-level summary comment of its blocked Epic.

This information is required by the Project Leads for their updates to the Oversight Committee. 

To illustrate, imagine an Epic modeling _DCP Support for 10X and SS2 Mouse data_ assigned to Quarter 3 Milestone 3. A child issue of this Epic slips from Milestone 1 to Milestone 2, but there is no impact on delivering the Epic in Milestone 3 as planned. No harm. No need to document. If the child issue slipped to a Milestone when it would impact the Epic - such as Quarter 4 Milestone 1, then it must be identified and documented because the Epic will be delivered late.

#### Checkpoint for Milestone 1
[Checkpoint for Milestone 1]: #check-point-for-milestone-1

 Further refinements for Milestone 2 are completely modeled. All Milestone 1 issues modeling Roadmap Objectives which are slipping to future Milestones MUST be identified and documented.

#### Checkpoint for Milestone 2
[Checkpoint for Milestone 2]: #check-point-for-milestone-2

 Further refinements for Milestone 3 are completely modeled. All Milestone 2 issues modeling Roadmap Objectives which are slipping to future Milestones MUST be identified and documented.

#### Checkpoint for Milestone 3
[Checkpoint for Milestone 3]: #check-point-for-milestone-3

All Milestone 3 issues modeling Roadmap Objectives which are slipping to future Milestones MUST be identified and documented.

#### Tracking Release Progress
Product Owners for each Objective MUST ensure that their Epics and child issues are kept up-to-date with current status.

Technical Leads MUST drive implementations of prioritized Epics forward and communicate blockers or changes in schedule if they arise.

Over-communication is strongly RECOMMENDED.

***Every two weeks***. The Release Shepherd facilitates a one hour meeting for Product Owners, User Experience, and Technical Leads to review the progress of the Release in ZenHub and address emerging issues.

The Release Shepherd MUST send a status update to the Project Leads on the DCP PM mailing list that captures action items for participants, documents slips in schedule, and/or requests clarification as needed.

As required, the Release Shepherd SHOULD add an agenda item to the recurring DCP PM meeting to address issues pending resolution. The Project Leads MUST never be surprised by the *state of play*.

***At Milestone Completion***. The Project Leads MUST update the Oversight Committee on the progress of the Release to ensure appropriate accountability.

#### Modeling progress with ZenHub Pipelines [*dcp* repository]

The Zenhub [*DCP* Workspace](https://app.zenhub.com/workspaces/dcp-5ac7bcf9465cb172b77760d9) restricts its workflow to a minimal set of Pipelines:

- New
- Icebox
- Product Backlog
- In Progress
- Closed

No other pipelines are available.

New items in the *dcp* repository appear in the *New* pipeline and MUST be triaged into the *Product Backlog* pipeline, the *Icebox* pipeline, or the *Closed* pipeline by the Release Shepherd or Product Owners.

The ZenHub *Icebox* pipeline SHOULD be used with great restraint in the *dcp* repository. It's being _grandfathered in_ only due to historical reasons. 

All Epics or issues included in a specific Release are initially assigned to the *Product Backlog* pipeline. Once the Epic (or any of its children) starts, then its *dcp* issue is assigned to the *In Progress* pipeline. When all the children of the Epic are in the *Closed* pipeline (meaning validated in Production), then its *dcp* issue is assigned to the *Closed* pipeline.

#### Modeling progress with ZenHub Pipelines [component repository]

In addition to the *DCP* workspace, component repositories MAY also be connected to a second [ZenHub Workspace with a customized set of Pipelines](https://www.zenhub.com/blog/announcing-zenhub-workspaces-personalized-workflows-for-every-team/) that more closely models the workflow or preferences of an institution or team. Repositories that are currently connected to the _DCP_ workspace can easily be connected to a second Workspace using the ZenHub UI [with no loss of state](https://help.zenhub.com/support/solutions/articles/43000484539-issue-placement-when-adding-repos-to-a-workspace-) such as the current set of Pipelines and the order of issues within a specific Pipeline.

This second Workspace MUST include a minimal set of common ZenHub Pipelines:

- New
- Product Backlog
- In Progress
- Closed

When an issue in a component repository is contained in an Epic in the _dcp_ repository the custom Pipelines in the second Workspace need to be mapped to the restricted set of Pipelines in the _DCP_ Workspace to allow progress to be tracked. 

To illustrate _mapping_, imagine that UCSC prefers finer granularity for tracking progress. A _UCSC_ Workspace is created and several Pipelines are added:

- _In Progress_ (being implemented)
- _Review_ (being reviewed)
- _Dev_ (observable in the `dev` deployment of the component)
- _Integration_ (observable in the `integration` deployment)
- _Staging_ (observable in the `staging` deployment)
- _Prod_ (observable in the `prod` deployment)

Since UCSC follows the Scrum process, a _Sprint Backlog_ Pipeline is also added.

For this case, the owner would assign its child issue of the _dcp_ Epic to the following Pipelines per Workspace as the issue progressed through the workflow:

| DCP Workspace Pipeline | UCSC Workspace Pipeline |
| -----------: | ---- |
| Product Backlog | Product Backlog |
| Product Backlog | Sprint Backlog |
| In Progress | In Progress |
| In Progress | Review |
| In Progress | Dev |
| In Progress | Integration |
| In Progress | Staging |
| In Progress | Prod |
| Closed | Closed |

UCSC has created this [Orange Workspace](https://app.zenhub.com/workspaces/orange-5d680d7e3eeb5f1bbdf5668f/board?milestones=Q3%202019%20Milestone%203%232019-09-23&releases=5ccb269f364bad566f9542d0&repos=139095537,124946326,124946282,130759918,198898884,93565055,168172092,142053167).

### Release Demonstration and Retrospective

***At Release Completion***. The Release Shepherd MUST schedule and faciliate a Release Demonstration and Retrospective meeting with the DCP Community and the Oversight Committee. 

The intent of the meeting is to demonstrate the Release deliverables as documented in the Roadmap Objectives, to share learnings, and identify opportunities to improve the planning process for future Releases. 

### Acceptance Criteria

This process will be under continuous community introspection and improvement through Retrospectives. If the process fails to deliver product increments with high confidence after three quarters with an annual Roadmap, then more radical alternatives SHOULD be pursued that satisfy the requirements from the Oversight Committee. We SHOULD also assess whether ZenHub is meeting our needs for a common project management application. 

### Unresolved Questions

*What aspects of the design do you expect to clarify further through the RFC review process?*

The authors are seeking recommendations for:
* Improving the accountability and confidence for on-time delivery of Roadmap Objectives during a Release. (In other words, what are the consequences for slipping the original Milestone or Release assigned to an Objective? This should not be friction-less)
* Assigning owners to Release Objectives with appropriate load balancing
* Identifying the minimal set of ZenHub pipelines (workflow) to be adopted across teams

### Drawbacks and Limitations

The adoption of a shared DCP community practice in preference to institutional practice requires compromise and adaptation which can be challenging.

### Prior Art

#### *Roadmaps*

 Examples that the Project Leads want to model are high-level, but descriptive themes from other open source or open science initiatives, such as the [*Rust Roadmap*](https://blog.rust-lang.org/2019/04/23/roadmap.html) or [*Ensembl’s Declaration of Intentions.*](http://www.ensembl.info/category/01-release/)

 The definition of a theme was generated by the DCP PM team at the April 2019 offsite during a heatmap/[*affinity mapping
exercise*](https://gamestorming.com/affinity-map/), where the core foundations of a theme resulted in the concepts of vision, user needs
and goals, collections, stakeholder requirements, long-term, and discrete. Further, themes contain the intentions for what will be solved to complete a customer need.

 #### *User Surveys*

[Rust Survey](https://blog.rust-lang.org/2018/11/27/Rust-survey-2018.html) 


### Alternatives

There are many potential methodologies and alternatives for project management. This RFC defines a minimal framework based on the experience of the authors.
