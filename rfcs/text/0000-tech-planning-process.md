### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# TL Quarterly Planning Process

## Summary

This document details the a quarterly process by which TLs (Technical Leads) will contribute to the overall DCP
quarterly roadmapping process via a Technical Objectives RFC, presented during the TL+PM Planning Call.

## Author and Shepherd

[Arathi Mani](mailto:arathi.mani@chanzuckerberg.com)

## Motivation

The new Roadmaps and Planning Process, detailed in the RFC [here](https://github.com/HumanCellAtlas/dcp-community/blob/4bb18c65af5b244460cf404465b1dbc4ee0cc65e/rfcs/text/0000-roadmaps%2Bplanning.md),
defines a process for annual roadmaps that inform quarterly release planning. TLs (Technical Leads) should contribute
“grassroots” knowledge about the technical requirements of the DCP on a quarterly basis to guide the 
[quarterly Release process](https://github.com/HumanCellAtlas/dcp-community/blob/4bb18c65af5b244460cf404465b1dbc4ee0cc65e/rfcs/text/0000-roadmaps%2Bplanning.md#planning-and-executing-a-release)
via a Technical Objectives RFC. This document details the contents of the Technical Objectives RFC as well as the
process that must occur such that this artifact is drafted and curated according to a timeline that will allow us to
present the artifact to the quarterly TL+PM objective curation meeting.

This proposed TL Quarterly Planning Process is **owned by the collection of TLs in the DCP** and is intended to
be a live process that iterates over time as required. The first iteration of the planning process is detailed below.  

Note: The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT
RECOMMENDED" "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14, RFC2119, and RFC8174
when, and only when, they appear in all capitals, as shown here.

### User Stories

As a TL of the DCP, I am able to surface important technical priorities to Product Management that will be incorporated
into the yearly roadmap. 

As a developer on the DCP, I am able to allocate enough time to work on high priority technical architectural issues
knowing that the work will facilitate quicker development of feature requests in the long term as informed by
the roadmap themes.

## Definitions
**Quarter:** One quarter is equal to 13 weeks. Generally the quarters have the following timeline:

- Quarter 1 (Q1): first Monday of January -> last Friday of March
- Quarter 2 (Q2): first Monday of April -> last Friday of June
- Quarter 3 (Q3): first Monday of July -> last Friday of September
- Quarter 4 (Q4): first Monday of October -> last Friday of December (before the holidays)

**Milestone:** a four week timeline/duration in a Release (sometimes five when a holiday is involved).

**Release:** composed of three Milestones and occur on a quarterly cadence.

**TL Roadmapping Shepherd:** Each quarter one TL acts as the “Shepherd” to make sure the TL planning process takes place
in time to produce the expected Technical Objectives RFC for presentation to the TL+PM planning meeting. The Shepherd
role will rotate once a quarter according to a spreadsheet drawn up post the approval of this RFC. 

**Technical Objectives RFC:** The final artifact that will be presented to the TL+PM planning call as a representation
of what TL would like to prioritize work on in the following quarter.

**Component:** Also known as a “Box” or a *chartered component*; represents a piece of the DCP led by a TL and worked on
by a team of software engineers. The list of chartered components are maintained
[here](https://github.com/HumanCellAtlas/dcp-community/tree/master/charters).


## Detailed Design

### Input to the TL Planning Process

1) Date of TL+PM planning call (information to be gathered by the TL Roadmapping Shepherd).

2) Epics/Objectives from previous quarter that will slip the release and rollover into the next.

### Timeline

Per the Roadmaps and Planning Process, a TL+PM call is expected to take place during the third week of Milestone 3 in a
Release quarter.

| &nbsp;&nbsp;Week in Milestone (Week in Quarter) &nbsp;&nbsp;| Deliverables |
|           :----:                    | :--- |
| **M2W3 (Week 7)**  | - Each component MUST enumerate the work they believe the DCP should prioritize in the following quarter, along with rough estimates of time (sized using T-shirt sizes) and specify any other activities that will also occur (e.g. onboarding new team members) and rough estimates of time. |
| **M2W4 (Week 8)**  | - A 1 hour TL meeting (the TL Coalescing meeting) MUST occur to consolidate the objectives that have been individually presented and any projects involving multiple components will be sized. |
|                    | - After the objectives have been coalesced, the union of all presented objectives (minus any objectives that were removed due to low priority) will be sent out to TLs to individually rank in priority. |
| **M3W1 (Week 9)**  | - A second 1 hour TL meeting (the TL Prioritizing meeting) will take place to bring together all TLs rankings to narrow down the objectives list to objectives that are high priority and feasible. |
| **M3W2 (Week 10)** | - The TL Planning Shepherd will turn the objectives agreed upon as representative of the TL priorities in the TL Prioritization meeting into an Information RFC and publish. This represents the final artifact: the **Technical Objectives RFC**. There will be no review period as the key stakeholders (the TLs) have already agreed upon the list. |
| **M3W3 (Week 11)** | - A 2 hour meeting MUST occur with TLs and PMs “to review any incomplete Objectives in the Release, the next Objectives in the DCP Roadmap, and the ***Technical Objectives RFC***.” |

**Note: MXWY is equal to “week Y in milestone X.”

**Note: this schedule may differ based on the holiday schedule each quarter. The responsibility is on the Shepherd of
the quarter to make sure the timeline is clear (especially the date of the TL+PM call) prior to the process commencing.

### Templates

For the purpose of curating the individual component objectives and for the consolidation of objectives into one that
represents all the components’ objectives, please use: 
[TL Objectives Template](https://docs.google.com/document/d/1jUo9Y7y9BJhVe8qpgljI45W7JpNlq3ownVBLA3wK0Qw/edit).

Once the consolidation has taken place, as well as the TL meeting in Week 9, the TL Roadmapping Shepherd MUST turn the
document into an Informational RFC and publish. The template for crafting an RFC for this purpose is 
[here](https://github.com/HumanCellAtlas/dcp-community/blob/4bb18c65af5b244460cf404465b1dbc4ee0cc65e/rfcs/roadmap-template.md) 
but should be tailored such that Themes are removed and replaced with the technical priorities.

### Specific Shepherd Tasks

1) Find out the date/week of the TL+PM planning call and post a timeline on when artifacts are expected to be completed
to Slack **#tech-architecture** so that folks are aware that the planning process has begun and expectations are set.

2) Schedule a 1 hour TL Coalescing Meeting and a 1 hour TL Prioritization Meeting, including all TLs\*, one and two
weeks after the individual components have been assigned to start working on their individual components’ objectives,
respectively.

3) If the meetings aren't separately scheduled, by default it will occur during the two consecutive Tech Architecture
meetings starting one week after components have been told to begin work on enumerating their objectives.

4) Transform the document produced at the TL Prioritization meeting into an RFC and publish.

5) **Annoyingly** send reminders about the timeline and expected artifacts and communicate consistently and frequently.
*If you aren’t annoying, you didn’t send out enough reminders.*

6) Shepherd along the TL Coalescing/Prioritizing meetings such that we get to a final set of priorities. Also act as the
primary representative of the TLs during the TL+PM meeting when needed.

\* If a TL is unable to attend the meeting due to unavoidable conflicts, a representative may attend instead.

### Unresolved Questions

- What do we do if the timeline slips and the TLs are not able to produce an artifact on time for the TL+PM call?
  - One suggested consequence: the roadmaps+planning RFC defines the outcome regardless of whether TL is contribute on
  time or not.
    
- How do we hold each component accountable to make sure they contribute to the planning process? The minimum
contribution expectation is that each component fills out the TL Objectives Template by the time the TL Coalescing
Meeting takes place.
