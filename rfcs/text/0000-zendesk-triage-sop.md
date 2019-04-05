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

### Immediate Need 
Beta 2 testing, to be done by 30-40 users representing three of our six use cases, will begin soon and we’ve decided to collect feedback through our Zendesk ticketing systm. Because this group of testers is larger and represents more diverse needs for data than the Beta 1 testers, we anticipate an up-tick in the number of Zendesk tickets we'll receive. We therefore need to quickly determine a process for handling these tickets. 

### Sustainability Through the GA
Upon general release, the full range of use cases will come into play and we anticipate a large increase in both the nubmer of tickets and diversity of questions asked by users. Addressing these tickets will require a big time committment, so we need an efficient system in place and a team well-versed in the DCP and its scientific applications. 

This RFC describes an SOP that we think will satisfy both the Beta2 test phase and the early days of the GA. As we grown in our understanding of user support needs, this SOP will likely need revision to address long-term sustainability.

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

![](../images/0000-Zendesk-triage-SOP-diagram.jpeg)

## Issue Creation

Issues in Zendesk originate from users of the DCP. These users could
include data contributors, researchers trying to use the data in the
DCP, software developers trying to leverage the DCP API. Any user of the
DCP can file an issue. 

While problems identified by an internal DCP staff member could be filed on Zendesk too, we favor a plan in which those issues are directly added to the applicable development
repository, where they can be addressed directly. This plan eliminates the extra work for the triager of managing internally-generated tickets.

Once an issue is
created in Zendesk there is a tiered support network that kicks in to
resolve the question.

## Primary Support

The triage person, or triage team, is responsible for receiving initial
support requests from end users, and monitors the Zendesk system daily
for new tickets. Note that the Zendesk system automatically sends out an initial canned response to the user, indicating that the request was received and is being reviewed by our support staff. 

The Service Level Objective is to respond to the user with a solution no later than 48 business hours after it is filed.

The triager takes ownership of each open ticket and does the following to develop a response:

1. Evaluate whether there is enough information in the ticket to inform the
response. If not, write back to the user and ask for the missing information. 

2. Search the data portal documentation and FAQ for answers to the question. If the triage person can find an answer that resolves the problem, a repsonse is sent to the user and the issue is closed. 

3. If an answer is not evident from searching the knowledge base in the data portal, the triager needs to ask for help from the secondary support team, which includes members from each DCP component or service. It may
be obvious which team should receive the ticket; if so, then it can be
assigned directly. If it’s not obvious, there can be some internal
discussion with the secondary support staff about who should receive the
ticket.

4. At this point the ticket becomes the responsibility of the secondary support person, who should repond to the user and bring the ticket to an eventual close. 


In order to improve our User Support the triager will also work with the secondary support team to:

1. tag each ticket with basic tags (data store/ingest/contribution/etc.) that help DCP staff categorize it for later retrieval. In addition, Github tickets should be made for issues like: updating docs or writing new documentation and improving our FAQs, code examples, bug reports, etc. As much as possible, issues should be turned into documentation or short FAQs and made available for users to access from the portal.

2. create and maintain canned responses (how to connect/get involved with the HCA;
how to submit data, etc.) that can be quickly sent to users who have written with those questions.

3. document discussions and decisions that arise from particular tickets, so that it's clear who should respond to similar questions in the future and what the response should be.

4. As much as possible 



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
