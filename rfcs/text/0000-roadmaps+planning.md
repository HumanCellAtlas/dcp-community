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

## Detailed Design for Roadmaps

The Product Roadmap creates a shared understanding across the DCP community and Users of the DCP for investments and direction for the next year. The Roadmap process MUST occur on an annual basis. 

Themes and Objectives from the Roadmap are the primary source of priorities for planning Releases each quarter.

### Timebox for Roadmaps

All timeboxes are noted as business days. Business days are included regardless of national holidays.

Overlapping timeboxes denote concurrent work.

Timeboxes with a range of days indicate that the work MAY be completed at any point during the timebox, but MUST be completed no later than the final day of the timebox.

**Note**: *Input from the Technical Architecture team is integrated on a quarterly basis during Release planning.*

Day 1 in the Roadmap timeline begins in the third quarter.

* **Day 1 - Day 30: [Gathering and reviewing input](#gathering-and-reviewing-input)**
  * Prerequisite: Science Roadmap
  * Prerequisite: Technical Objectives RFC
  * Day 1-30: DCP User Survey 
  * Day 1-10: Roadmap Retrospective
* **Day 1 - Day 45: [Drafting the roadmap](#drafting-the-roadmap)**
  * Day 1-10: Identify a Roadmap Shepherd
  * Day 30-35: Create themes
  * Day 36: Collate themes
  * Day 37-45: Refine themes
* **Day 45 - Day 60: Receiving feedback**
  * Day 45: Publish draft Roadmap
  * Day 46-55: Community Review period 
  * Day 56-60: Complete final edits
* **Day 60 - Day 70: [Iterating with Oversight](#iterating-with-oversight)**
  * Day 60: Send Roadmap to Oversight
  * Day 61-70: Review and edit roadmap with Oversight
  * Day 70: Receive Roadmap approval from Oversight
  * Day 70: Communicate approved Roadmap 


### Gathering and Reviewing Input
[Gathering and Reviewing Input]:#gathering-and-reviewing-input

#### Science Roadmap

Oversight MUST provide an annual Science Roadmap to Project Leads to be reviewed at the next Oversight meeting following its receipt.

Project Leads MUST clarify outstanding questions about the Science Roadmap with Oversight within two weeks.

#### Technical Objectives RFC

Project Leads MUST review the [Technical Objectives RFC](#technical-architecture-objectives) from the most recent quarter to understand outstanding technical concerns.

#### DCP User Survey and UX Insights

User Experience (UX) MUST create a DCP user survey on an annual basis to collect input for the Product Roadmap. The audience for the survey are the Users of the DCP.

UX MUST review the proposed survey with the DCP community and incorporate suggestions as appropriate.

The survey MUST be announced to the membership of the Human Cell Atlas (through its Registration mailing list) with a minimum of a two week last call for responses.

At the conclusion, UX MUST summarize insights from the survey and present at the next DCP PM meeting. Following a one week review to incorporate feedback, the survey MUST be shared by UX with the membership of the Human Cell Atlas and announced on the *HumanCellAtlas #dcp* slack channel.

UX MAY also share critical UX Insights from other sources independent of the Survey for consideration by the Project Leads during the creation of the Roadmap.

#### Roadmap Retrospective

The Roadmap Retrospective occurs during the recurring Project Leads meeting and not in a special session. The Roadmap Shepherd MUST add the Retrospective to the agenda and faciliate.

In preparation, the Project Leads MUST independently review the prior year’s Product Roadmap, Release Retrospective summaries, and appropriate supporting documentation and then share their significant findings and learnings with their peers during the Retrospective.

Findings may include: Themes that were incomplete and the accompanying reason, incorrectly sized work in the Engineering Plan, etc. These findings SHOULD be utilized as a reference class for the next Product Roadmap.

Project Leads MUST identify a Roadmap Shepherd during the Retrospective. The Shepherd MUST NOT be a Project Lead, to ensure the neutrality of the content for the Roadmap.

The Shepherd is responsible for:
* Managing the timeline for the creation of the draft Roadmap
* Facilitating all meetings related to the creation of the draft Roadmap 
* Documenting important decisions, discussions, and questions 
* Assigning action items to move toward the creation of the Roadmap, should no Project Lead volunteer, and following up with them through completion

### Drafting the Roadmap
[Drafting the Roadmap]:#drafting-the-roadmap

Project Leads MUST independently create Themes by utilizing the input provided in the Science Roadmap, Technical Objectives RFC, DCP User Survey and UX Insights, and Roadmap Retrospective.

To create the draft Product Roadmap, the Roadmap Shepherd:
- Forks the *HumanCellAtlas dcp-community* repository
- Copies `rfcs/roadmap-template.md` to `rfcs/text/0000-roadmap-YYYY.md` - where "YYYY" is the target year for the roadmap. 

The Roadmap Shepherd MUST collate all themes into the draft which is an Informational RFC. 

Project Leads MUST review the Themes as a collective and distill the revised list to a lesser number of themes. This is achieved through a consensus building process, which is implemented by the Roadmap Shepherd and occurs during specifically noted Project Lead meetings, outlined below:

* Explore the Themes, search for commonalities, and document concerns and issues 
* Identify potential consolidated or alternative Themes, resolution for concerns and issues, and action items required for further understanding
* Prioritize the Themes within the Roadmap
* Update the Roadmap to reflect revised content
* Resolve outstanding questions and concerns

If the Project Leads fail to reach [rough consensus](https://tools.ietf.org/html/rfc7282) within the specified timebox, then the draft MUST be escalated to Oversight for consultation.

When consensus is reached, the Roadmap is submitted as a pull request to the *dcp-community* repo and follows the DCP RFC process for a *Governance* RFC starting with [community review](https://github.com/HumanCellAtlas/dcp-community/blob/master/rfcs/text/0001-rfc-process.md#reviewing-rfcs). 

The Roadmap Shepherd MUST be assigned as the Shepherd for the RFC and ensure that the Project Leads (Authors) respond to comments, answer questions, and update the Roadmap RFC as required.

### Iterating with Oversight
[Iterating with Oversight]: #iterating-with-oversight

Roadmaps MUST be approved by Oversight rather than DCP PM which is a minor variation from the current RFC Approval process for Governance RFC(s).

The Roadmap Shepherd MUST send the draft Roadmap to Oversight a week in advance of a review meeting between the Project Leads and Oversight.

The Project Leads MUST iterate with Oversight to review and revise the draft Roadmap until it meets with Oversight approval.

Once approved, the Roadmap Shepherd will follow the normal RFC process for [approved RFC(s)](https://github.com/HumanCellAtlas/dcp-community/blob/master/rfcs/text/0001-rfc-process.md#approving-rfcs).

### Updating the Roadmap

The Roadmap MAY be updated with Project Lead consensus and Oversight approval for a number of reasons including but not limited to:

* A Theme is to be added or deleted.
* Additions or revision to Objectives change the original intent of the Theme.
* Emerging conditions require substantive changes to the original Roadmap direction.

Requests to update the Roadmap MUST be forwarded to the Roadmap Shepherd with a detailed description of the proposed changes, including the rationale for the updates, a timeline, and priority.

The Roadmap Shepherd MUST add the request to the next Project Leads meeting agenda for review.

The Roadmap Shepherd and Project Leads MUST review the update request and determine if there is rough consensus.

If consensus is reached, the Roadmap Shepherd MUST refresh the Roadmap and send to Oversight. 

Oversight MUST review the changes and approve.

The Roadmap Shepherd MUST update the previously published Roadmap RFC in the *dcp-community* repo and announce its approval in a similar fashion to the original RFC approval.

## Detailed Design for Planning and Execution

### ZenHub for Project Management

The [ZenHub](https://www.zenhub.com/) project management application has been partially adopted in the DCP community. This RFC fully commits the community to ZenHub as our *one source of truth* for project management and engineering plans.

DCP SHOULD NOT duplicate the ZenHub DCP Board in other documents or spreadsheets. The Board and its reports represent the definitive list of DCP work items for a Release and their current state. 

### DCP Product Backlog

The DCP ZenHub board is the Product Backlog for DCP. All DCP component repos that will assign items to a DCP Release and Milestone MUST be added to the ZenHub *DCP Backlogs* workspace. The [DCP PM Team](https://github.com/orgs/HumanCellAtlas/teams/dcp-pm-team) in the Human Cell Atlas GitHub organization MUST have write access to all the repo(s) in the *DCP workspace*.

The top-level *dcp* repo MUST contain **all** significant work that is important to the DCP community and requires visibility and prioritization. This includes Roadmap objectives and product-wide technical requirements (e.g. “components should have logs...“). 

If Institutions are using other project management applications such as Jira for internal requirements and reporting, then all DCP related issues in a Release MUST be *projected* into ZenHub for transparency and finer granularity for tracking progress.

To avoid maintenance overhead due to duplicate or incomplete issues, the creation of new items in the *dcp* repo SHOULD be limited to the PM Team or a facilitator from the Architecture Team.

DCP Product Backlog items are modeled as User Stories, Epics, and Spikes. 
 
#### Owners

A DCP owner MUST be assigned to each issue in the Product Backlog in the *dcp* repo to coordinate progress and drive the issue to on-time closure. This owner is responsible for ensuring that related issues are created in component repo(s) and linked to Epics within the *dcp* repo.

For example, an implementation task for a specific component resides in its own repo and is linked to the relevant Epic in the *dcp* repo.

#### User Stories

User stories are often in the form of simple templates such as:

`As a <type of user>
I want to <action>
so that <result>`

DCP issues SHOULD include a User Story, Acceptance Criteria ("Definition of Done"), and a Deliverable to be demonstrated during the Release Retrospective to Oversight.

The `<type of user>` MUST be specific and reference [HCA Personas](https://docs.google.com/presentation/d/1bsu8q9CzRXv3Y8c1p8NENi3p8KlxC4scp2tJkHO3Jeo/edit#slide=id.g3cf4e46e8f_0_38) when appropriate.

*As a data wrangler working on the DCP, I want to adapt the metadata schema quickly in order to respond to developments in experimental approaches and new assays.*

*Acceptance Criteria*
- *[ ] This*
- *[ ] That*
- *[ ] And one more thing*

*Deliverable that can be demonstrated to Oversight*

...

### Technical Architecture Objectives 
[Technical Architecture Objectives]:#technical-architecture-objectives

Through a process of their own choosing, the Technical Architecture team MUST document critical architectural issues and technical debt in a Technical Objectives Informational RFC for consideration during each Release. The RFC SHOULD outline technical strategies and estimate cost (such as two engineers at 20%) to realize its objectives. 

The RFC MUST be approved prior to [Clarifying the Next Release](#clarifying-the-next-release) for consideration.


### Planning and Executing a Release

The DCP community members SHOULD respond to requests related to Release planning and execution in a timely manner. If there is a failure to engage and be responsive, then a gentle escalation to the DCP PM slack channel is RECOMMENDED to ensure that progress occurs and the schedule does not slip. 

#### Timebox for Planning and Execution

Work MAY be completed at any point during the timeboxes below but MUST be completed no later than the final day of the timebox.

Release planning begins in the third Week of the third Milestone of the current Release. For example Q4 planning begins in Q3M3W3 and continues into Q4M1. To reiterate, Planning continues into Milestone 1 of a Release as needed.

* Prerequisite: Product Roadmap
* Prerequisite: Technical Objectives RFC
* Prerequisite: UX Insights
* Previous Release Milestone 3 Week 3: [Clarifying the Next Release](#clarifying-the-next-release)
* Next Release Milestone 1 Week 1: [Sketching the Release](#sketching-the-release)
* Next Release Milestone 1 Week 2: [Creating the Engineering Plan](#creating-the-engineering-plan)

#### Clarifying the Next Release
[Clarifying the Next Release]:#clarifying-the-next-release

***Previous Release Milestone 3 Week 3.*** The Roadmap Shepherd schedules a two-hour meeting with DCP PM and the Technical Leads to review any incomplete Objectives in the Release, the next Objectives in the DCP Roadmap, critical UX Insights, and the Technical Objectives RFC.

Incomplete Objectives are the highest priority for the next Release unless emerging conditions, as identified by the Project Leads, have altered the priorities for the previous Release plan.

The outcome from this meeting is the proposed set of Product and Technical Objectives for the next Release. One (and only one) Product Owner MUST be assigned to each proposed Objective. This Product Owner is responsible for refining and driving the Objective to on-time closure in coordination with Technical Leads and other collaborators.

The dates for the next ZenHub Release and Milestones MUST be assigned or updated in this meeting.

DCP PM MUST assign a rotating Release Shepherd to help the Product Owners and Technical Leads observe the processes and practices for the Release as outlined in this RFC. 

#### Sketching the Release
[Sketching the Release]:#sketching-the-release

***Next Release Milestone 1 Week 1.*** For each Objective in the Release, its Product Owner MUST create one or more Epics in the *dcp* repo that document User Stories, Acceptance Criteria, and the Deliverable to Demonstrate.

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

It is strongly RECOMMENDED that engineering teams reserve a **minimum of 30%** capacity during a Release to account for:

*  Emerging security or operational issues 
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
1. Document and monitor the *work-in-progress*. If an RFC is required, a reference to the draft RFC in the *dcp-community* repo is required; otherwise a reference to other documentation is appropriate.
(For designs, it is RECOMMENDED that a temporary public working group channel be created in *HumanCellAtlas* Slack and documented in the Spike to ensure that the *state of play* is visible to the DCP community.)
1. Document its Acceptance Criteria such as an approved RFC. 
1. Add the Spike to its related Epic.
1. Add the Spike as a blocking dependency in its related Epic.
1. Assign the Spike to a specific Milestone in the Release. Spikes have a finite duration.

The related Epic cannot be estimated and refined until the completion of its blocking Spike. Once the Spike is closed, then its Epic MUST be modeled as outlined in [Creating the Engineering Plan](#creating-the-engineering-plan).


#### Modeling Objectives that Require Multiple Releases

Objectives which span multiple Releases MUST be decomposed into one or more Epics per Release. The Epics MUST be assigned to a Release and linked as blocking dependencies. For example, *Epic 1 in Release 1 blocks Epic 2 in Release 2 blocks Epic 3 in Release 3* which clearly documents their dependency chain and schedule.

An RFC is strongly RECOMMENDED for Objectives that require multiple releases to complete.

#### Creating the Engineering Plan
[Creating the Engineering Plan]:#creating-the-engineering-plan

***Next Release Milestone 1 Week 2***. In consultation with their relevant Technical Leads and collaborators, the Product Owner MUST complete the refinement and modeling for *well-understood* Epics, including the addition of child issues for product readiness and the surfacing of dependencies between software components using ZenHub Dependencies.

Milestones MUST be assigned to all issues assigned to the Release not blocked by a Spike.

It is RECOMMENDED that caution be exercised when scheduling issues in a dependency chain in the same Milestone. Historically, this has been the source of slips in DCP schedules.

The Release Shepherd MUST validate accurate modeling of Objectives for the Release in ZenHub and create a Release epic named “Q#YYYY” such as “Q42019” which contains all the related *dcp* Epics and issues for the Release. This will automatically include all child issues in other component repo(s).

This Epic represents the Engineering Plan which MUST be sent to the DCP PM mailing list for review and approval, noting any Objectives that were cut or reduced in scope due to engineering capacity. The Project Leads MUST respond to the original message within two days and MAY negotiate changes to the Engineering Plan. 

***Next Release Milestone 1 Week 3***. Further refinements for Milestone 2 are completely modeled. All Milestone 1 issues that will slip to Milestone 2 MUST be identified.

***Next Release Milestone 2 Week 3***. Further refinements for Milestone 3 are completely modeled. All Milestone 2 issues that will slip to Milestone 3 MUST be identified.

***Next Release Milestone 3 Week 3***.
All Milestone 3 issues that will slip to the next Release MUST be identified.

#### Tracking Release Progress
Product Owners for each Objective MUST ensure that their Epics and child issues are kept up-to-date with current status.

Technical Leads MUST drive implementations of prioritized Epics forward and communicate blockers or changes in schedule if they arise.

Over-communication is strongly RECOMMENDED.

***Every two weeks***. The Release Shepherd facilitates a one hour meeting for Product Owners, User Experience, and Technical Leads to review the progress of the Release in ZenHub and address emerging issues.

The Release Shepherd MUST send a status update to the Project Leads on the DCP PM mailing list that captures action items for participants, documents slips in schedule, and/or requests clarification as needed.

As required, the Release Shepherd SHOULD add an agenda item to the recurring DCP PM meeting to address issues pending resolution. The Project Leads MUST never be surprised by the *state of play*.

***At Milestone Completion***. The Project Leads MUST update Oversight on the progress of the Release to ensure appropriate accountability.

#### Modeling progress with ZenHub Pipelines [*dcp* repo]

New items in the *dcp* repo appear in the ZenHub *New* pipeline and MUST be triaged into the *Product Backlog* pipeline, the *Icebox* pipeline, or the *Closed* pipeline by the Release Shepherd or Product Owners.

The ZenHub *Icebox* pipeline SHOULD be used with great restraint in the *dcp* repo. Its explicit purpose is to queue up potential futures from the DCP roadmap such as pending DevSecOps issues. Individual component repo(s)  MAY use the *Icebox* pipeline in a manner of their own choosing.

No issues in the *dcp* repo MUST be assigned to the *Epic* or *Sprint Backlog* pipelines. Individual component repo(s) MAY use these pipelines in a manner of their own choosing.

At the level of the *dcp* repo, a subset of ZenHub pipelines are used to track progress. All Epics or issues in a Release are initially assigned to the *Product Backlog* pipeline. Once the Epic (or any of its children) is started, then its *dcp* issue is moved to the *In Progress* pipeline. When all the children of the Epic are in the *Closed* pipeline, then the *dcp* issue is moved to the *Closed* pipeline.

#### Modeling progress with ZenHub Pipelines [component repo]

When an issue is started, it is moved to the *In Progress* pipeline.

When a pull request is filed and in review for an *In Progress* item, the issue is moved to the *Review* pipeline.

If the pull request is rejected, the issue returns to *In Progress*; otherwise, it is merged and the related issue moves to the *Merged* Pipeline.

When all changes are verified in [Production](https://allspark.dev.data.humancellatlas.org/dcp-ops/docs/wikis/SOP:%20Releasing%20new%20Versions%20of%20DCP%20Software), the issue is moved to the *Closed* pipeline.

### Release Demonstration and Retrospective

***At Release Completion***. The Release Shepherd MUST schedule and faciliate a Release Demonstration and Retrospective meeting with the DCP Community and Oversight. 

The intent of the meeting is to demonstrate the Release deliverables as documented in the Roadmap Objectives, to share learnings, and identify opportunities to improve the planning process for future Releases. 

### Acceptance Criteria

This process will be under continuous community introspection and improvement through Retrospectives. If the process fails to deliver product increments with high confidence after three quarters with an annual Roadmap, then more radical alternatives SHOULD be pursued that satisfy the requirements from Oversight. We SHOULD also assess whether ZenHub is meeting our needs for a common project management application. 

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
