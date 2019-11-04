### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# TL Quarterly Planning Process

## Summary

This document details the a quarterly process by which TLs (Technical Leads) will contribute to the overall DCP quarterly Product Roadmap refresh process via a Technical Objectives RFC.

## Author and Shepherd

[Arathi Mani](mailto:arathi.mani@chanzuckerberg.com)

## Motivation

The Roadmaps and Planning Process, detailed [here](https://github.com/HumanCellAtlas/dcp-community/blob/master/rfcs/text/0012-roadmaps%2Bplanning.md#refreshing-technical-objectives), defines a process for the development of an annual Product Roadmap in order to deliver product increments on the DCP. This Product Roadmap is refreshed on a quarterly process by the Product Management (PM) team to reflect any emerging requirements and as part of the refresh cycle, the Technical Leads (TLs) are [required to contribute a set of objectives](https://github.com/HumanCellAtlas/dcp-community/blob/master/rfcs/text/0012-roadmaps%2Bplanning.md#refreshing-technical-objectives) that reflect critical architectural and technical priorities that should be worked on in the upcoming quarter. This document details the contents of the Technical Objectives RFC as well as the process that must occur such that this artifact is updated each quarter in time for the refresh.

Note: This proposed TL Quarterly Planning Process is **owned by the collection of TLs in the DCP** and is intended to be a live process that iterates over time as required. The first iteration of the planning process is detailed below.

### User Stories

As a TL of the DCP, I am able to surface important technical priorities to Product Management that will be incorporated into the yearly roadmap.

As a developer on the DCP, I am able to allocate enough time to work on high priority technical architectural issues knowing that the work will facilitate quicker development of feature requests in the long term as informed by the roadmap themes.

## Definitions

**Milestones** represent a four week timeline/duration in a Release.

**Releases** are composed of three milestones and occur on a quarterly cadence.

**Technical Objective:** a high level goal that encapsulates a general area of DCP technical work that can be flexibly broken up by specific sub-tasks. Sub-tasks are usually represented as Epics. More details on the definition is below.

**Technical Objectives RFC:** The final artifact that will be consulted by the PM team for the roadmap refresh, with the expectation that the objectives will be prioritized for the following quarter, following spirited discussion as needed. 

## Detailed Design

### What is a Technical Objective?

A Techincal Objective is a body of work for the DCP, usually cross-cutting, related to:

1) Scalability and Performance
2) Reduction of code debt to improve reliability
3) Deprecation (or migration) of technologies used in the DCP
4) Refactoring work in order to make modifications to the DCP Architecture (usually related to points 1 or 2).

A Technical Objective is NOT:

1) A “feature” related to the DCP product. If a TL deems that some feature is missing from the Product Roadmap, it is the responsibility of the TL to communicate with someone on the PM team to ensure that the feature's need is brought up in PM discussions.
2) Related to DevSecOps or Compliance. Objectives that fall under either of these categories include anything related to CI/CD, security, and Legal (e.g. GDPR). The DevSecOps team and the Compliance working groups are responsible for establishing these objectives for the Product Roadmap.

### Process

Per the [Roadmaps and Planning Process Timeline](https://github.com/HumanCellAtlas/dcp-community/blob/master/rfcs/text/0012-roadmaps%2Bplanning.md#timeline-for-roadmaps), a the Technical Objectives RFC is expected to be completed by the end of Milestone 2, Week 1 (M2W1) which informs the timeline detailed below.

| Week in Milestone | Deliverables |
|           :----:                    | :--- |
| **M1W1**  | A Google Doc is sent out (either the previous document used from a previous quarter or a fresh document) to the Technical Architecture team in order to solicit input on what are deemed to be important technical areas of improvement that should be prioritized in the upcoming quarter. The input to this document is free-form and TLs may or may not choose to participate, with the understanding that if no participation is given, no objectives will be generated. |
| **M1W2** | The Technical Architecture Chairperson (Chair) will spend the week coalescing the free-form text into concrete objectives that will be presented in the Technical Architecture meeting. A discussion will ensue to form consensus around the objectives and if no consensus is met, the entire set of objectives will be included in the RFC for further debate during the RFC's community review. |
| **M1W3** | The Technical Objectives RFC is updated by the Chair and sent out for the required 2 week community review, following which the changes may be pushed and the RFC may be used as final by the PM team. |

**Note: MXWY is equal to “week Y in milestone X.”

**Note: This schedule may differ based on the holiday schedule each quarter. The responsibility belongs to the Chair to ensure that the timeline is clear (especially the due date of the TL Objectives RFC) prior to beginning the process.

### Specific Chair Tasks

1) Find out the due date/week of the TL Objectives RFC and post a timeline on when tasks are expected to be completed to Slack **#tech-architecture** so that folks are aware that the planning process has begun and expectations are set.

2) Post the Google Doc for Technical Architecture free-form input to the **#tech-architecture** Slack channel.

3) Coalesce the free-form text into meaningful and achievable Technical Objectives. Present the objectives during the following Technical Architecture meeting.

4) Update the Technical Objectives RFC with the debated/curated objectives and shepherd the RFC process to completion. 

5) Once the RFC is published, act as the POC to answer any questions the PM team might have around the objectives.

6) To maintain the flexibility iterability of this process, if the Chair notices that the process detailed in this RFC is not working, the Chair is free to adapt the process as needed during that quarter to generate the required input into the roadmap. This adaptation should be documented and this process should be edited accordingly.
