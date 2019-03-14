### DCP PR:
***Leave this blank until the RFC is approved then the Author(s) must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc\#\](https://github.com/HumanCellAtlas/dcp-community/pull/\<PR\#\>)`

# Technical Support SOP

## Summary

This process RFC describes how the DCP handles technical support requests through the use of the Zendesk application and other processes.

## Author(s)

`[Kevin Osborn](mailto:kosborn2@ucsc.edu)`

`[Jodi Hirschman](mailto:jhirsch@broadinstitute.org)`

`[Matthew Spier](mailto:mspeir@ucsc.edu)`

`[Mallory Freeberg](mailto:mfreeberg@ebi.ac.uk)`

## Shepherd

***Leave this blank. This role is assigned by DCP PM to guide the Author(s) through the RFC process.*

## Motivation

Beta 2 testing will begin soon and we’ve decided to collect feedback
through our Zendesk ticketing system. We therefore need to quickly
determine a process for handling larger numbers of Zendesk tickets than
we have received to date. Several of us have been thinking about this
for awhile and drew up an initial SOP that we refined at the last DCP
F2F (Feb. 2019); we present it here as this RFC. 

### **User Stories**

1.  > As a researcher new to the DCP, I would like to get a timely
    > answer to my question so that I can have a successful experience
    > submitting, exploring, downloading, or analyzing HCA data.

2.  > As a DCP PM, PO or PL, I would like to be assured that Zendesk
    > tickets are being responded to in a timely manner so that I can
    > focus on my own tasks without having to micromanage responses.

3.  > As a DCP team member, I would like to know whether or not I should
    > be responding to a Zendesk ticket so that I can either respond in
    > a timely manner or be assured that someone else is responding in a
    > timely manner.

## Scientific "guardrails" \[optional\]


## **Detailed Design**

This section describes the flow of an issue through the support state
diagram presented below.

![](media/image1.jpg)

## Issue Creation

Issues in Zendesk originate from users of the DCP. These users could
include data contributors, researchers trying to use the data in the
DCP, software developers trying to leverage the DCP API. Any user of the
DCP can file an issue. Problems identified from the internal DCP staff
can be filed on Zendesk too, or directly in the applicable development
repository if the reporter is sure of which one to use. Once an issue is
created in Zendesk there is a tiered support network that kicks in to
resolve the question.

## Primary Support

The triage person, or team, is responsible for receiving the initial
support requests from end users, and monitors the Zendesk system daily
for new tickets. The triager takes ownership of each open ticket, sends
out a canned response noting that we’ve received the user’s message, and
looks to see if there is enough information in the ticket to inform the
response. They will go back to the user for any obvious omissions. Next,
the triage person will look through the knowledge base for answers to
known problems. Note that the knowledge base is available to end users,
and includes the documentation and also FAQs accessible from the Help
page. If the answer resolves the problem then the issue is closed. If
not, then the triager needs to ask for help from the secondary support
team, which includes members from each DCP component or service. It may
be obvious which team should receive the ticket; if so, then it can be
assigned directly. If it’s not obvious, there can be some internal
discussion with the secondary support staff about who should receive the
ticket.

The triage process will also include tagging the question with basic
tags (data store/ingest/contribution/etc.). The secondary support person
to whom the triager asks for help is encouraged to add additional tags
and to make Github tickets for issues like: new/updated docs,
new/updated FAQs, code examples, bug reports, etc.

The triager(s) will also work with the secondary support team to create
and maintain canned responses (how to connect/get involved with the HCA;
how to submit data, etc.)

The Service Level Objective is to respond to the initial problem
submission no later than 48 business hours after it is filed.

## Secondary Support

The secondary support team is comprised of 1-2 members from each major
component or service of the DCP. These representatives should have
enough technical knowledge to answer most questions. Answers generated
by the secondary support team will be added to the knowledge base. They
may require additional information from the person who created the
ticket. They also may need to consult with technical experts who make up
the tertiary support teams.

## Tertiary Support

The Tertiary team is the entire DCP team. In the event that the
secondary support team is not able to respond to the question, anyone
may be called on to answer it. However, the need for Tertiary support
should be rare, and all answers will be archived and added to the
knowledge
base

### **Acceptance Criteria \[optional\]**

*Acceptance criteria are the conditions that a RFC must satisfy to be accepted by users or other stakeholders.*

### **Unresolved Questions**

We suggest that funding be sought to hire one or two part or full-time
triagers who would grow into the position as we move toward GA, and be
able to handle primary triage as well as some of the more difficult
questions by the time of the GA, when we expect support needs to
increase significantly.

> 2 triage agents - could be 50% of their job as they would trade off
> being point for this job. Duties would include the grooming of the FAQ
> knowledge base. They would also be responsible for moderate our user
> forum.
> 
> 1 point person per box (4-5 people) - this should be at least 25% of
> their time, and there should be a backup person for each box


### **Drawbacks and Limitations \[optional\]**

*Why should this RFC not be implemented?*

### **Prior Art \[optional\]**

*Share references to prior art to deepen community understanding of the RFC, such as learnings, adaptations from earlier designs, or community standards.*

### **Alternatives \[optional\]**

*Highlight other possible approaches to delivering the value proposed in this RFC. What other designs were explored? What were their advantages? What was the rationale for rejecting alternatives?*
