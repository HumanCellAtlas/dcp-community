### DCP PR:

PLEASE DO NOT REVIEW! THIS RFC IS NOT COMPLETE AND IS STILL ACTIVELY BEING WORKED ON.

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# RFC Name

*Replace "RFC Name" above with the name of your RFC. Keep it short and descriptive.*

## Summary

The Data Coordination Platform (DCP) established the Request For Comments (RFC) process on 
[September 17th 2018](https://github.com/HumanCellAtlas/dcp-community/pull/26) in order to address the gap of having a
coordinated, standardized, and repeatable process by which decisions could be made. Since this time, the DCP has evolved
and likewise the RFC process is in need of some fine tuning. This RFC proposes some changes in order to reflect the new
DCP community's needs.  

## Author(s)

*Recommended format for Authors:*

 [Arathi Mani](mailto:arathi.mani@chanzuckerberg.com)
 
 [Dave Rogers](mailto:dave@clevercanary.com)

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

Over the last year since the original RFC process was accepted, the DCP project has grown both in size and complexity.
As of this writing, 11 RFCs have been accepted in addition to many charters. Not so unexpectedly, there have been pain
points along the way of which we try to address a few via edits to the RFC process defined below.


### User Stories

*Share the [User Stories](https://www.mountaingoatsoftware.com/agile/user-stories) motivating this RFC.*

## Detailed Design


### 1. Add additional RFC states

Currently the states of an RFC is one of:
 
 1. "rfc-community-review",
 2. "rfc-oversight-review", 
 3. "rfc-approved", or
 4. "rfc-declined." 

In practice, there have been cases where an RFC is no longer implemented (i.e. "deprecated") or an RFC has been
withdrawn due to either incompleteness or having been published too early. 

Additionally, with the encouraged behavior of submitting draft pull requests prior to RFC readiness for review, there is
no way to clearly designate the fact that the RFC should not be reviewed at all until the PR is no longer a draft.

We propose adding the following new tags: 

**rfc-draft** : This indicates that the authors are still actively working on the RFC, that the RFC is in a draft state,
and readers should **not** comment on the RFC yet. Additionally this state serves to indicate more clearly the backlog
of RFCs in development to avoid duplication of effort of overlapping RFCs and to help elicit offers of co-authorship.

**rfc-deprecated** : This indicates that the design is no longer implemented or executed in the DCP and has been usurped
by a newer design or process. Note that the deprecated tag is not standalone; it may be used in conjunction with 
"rfc-approval" tag for example.

**rfc-paused** : This indicates that an RFC has been paused for further refinement. This roughly implies that an RFC has
gone from being in a review status to a "pre-rfc" status and should not be reviewed further until designated so.

**rfc-withdrawn** : This tag is applied when an RFC has not finished the review process (and therefore has not been 
explicitly rejected) but has been voluntarily withdrawn.

Like the existing RFC process, any change of state to the RFC must be announced on the DCP slack channel. Also note that
tagging an RFC has no implications on whether the PR is open or closed. Generally, RFCs in a terminal state (i.e.
"approved", "declined", or "withdrawn").

### 2. Reduce the RFC minimum review time

The current RFC process requires a minimum of 2 weeks in DCP Community Review followed by 1 week of DCP Oversight
Review. The intention of the "Community" review is to receive feedback not from other DCP team members, but rather the 
scientific community.

In practice, most RFCs are reviewed as a free-for-all during the community review period and then is followed by one
week of radio silence during the "Oversight" review period since almost all feedback will have been resolved during the
community review period.

We propose removing Oversight review altogether. A two week minimum community review period still exists but the 
"community" refers to all members of the DCP working on any of the chartered organizations instead of the scientific
community only. This largely reflects the behavior of the review process today.

Extensions to the two week minimum will remain the same as dictated by the existing RFC process at the discretion of
the author(s) of the RFC.

### 3. Add an explicit process to make "trivial" edits to published RFCs

The original RFC process does not explicitly mention that minor edits (i.e. typos, re-wordings, etc.) are free to be
pushed whenever needed. 

We propose explicitly stating that if an RFC warrants minor/trivial edits, such edits may occur at any time by the
author(s) of the RFC without announcement.

### 4. Elucidate RFCs' implementation status

Today, there is no requirement that RFCs must be related to any existing ticket in Zenhub which is usually a proxy for
determining critical work to be done in a quarter. This means that, at times,  RFCs are published/approved with no clear
roadmap to completion which also weakens the implication of the RFC.

We propose that, in order to ensure that RFCs are created based on critical tickets that are prioritized based on
the quarterly and yearly roadmaps and ensure that RFCs are followed up to completion, that all 
[*non informational RFCs*](https://github.com/HumanCellAtlas/dcp-community/issues/30) be linked to an existing Zenhub
ticket. Such a ticket should be linked to a timeline for RFC completion.

In addition, we propose adding an additional *optional* section to the RFC template: Timeline. This will allow the
authors to propose a timeline for the work to be completed thus informing others of any conflicting priorities.


### 5. Make RFC authorship and RFC status more explicit in Zenhub
Currently, very few RFC authors assign themselves the RFC PR in Github. This means that folks glancing though the
current open RFCs must open the RFC to see who is driving it. Furthermore, it makes it difficult to filter by author.

Additionally, the status of the RFCs is maintained by tags rather than by a Zenhub column making the backlog of each
type of RFC making it more difficult to see at a glance. 

We propose to explicitly request that authors claim their RFCs by assigning themselves as authors in Github. 

Additionally it is proposed to setup setup a custom Zenhub board with columns for each RFC state while maintaining tags
to indicate the disposition at the terminal (closed) state. 

### Unresolved Questions

- *What aspects of the design do you expect to clarify further through the RFC review process?*
- *What aspects of the design do you expect to clarify later during iterative development of this RFC?*

### Prior Art

[The OG RFC Process](https://github.com/HumanCellAtlas/dcp-community/blob/master/rfcs/text/0001-rfc-process.md)
