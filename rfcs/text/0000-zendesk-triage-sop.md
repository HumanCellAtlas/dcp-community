### DCP PR:
***Leave this blank until the RFC is approved then the Author(s) must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc\#\](https://github.com/HumanCellAtlas/dcp-community/pull/\<PR\#\>)`

# User Support SOP

## Summary

This process RFC describes how the DCP handles user support requests through the use of the Zendesk application and other processes.

## Author(s)

[Kevin Osborn](mailto:kosborn2@ucsc.edu)

[Jodi Hirschman](mailto:jhirsch@broadinstitute.org)

[Matthew Spier](mailto:mspeir@ucsc.edu)

[Mallory Freeberg](mailto:mfreeberg@ebi.ac.uk)

## Shepherd

***Leave this blank. This role is assigned by DCP PM to guide the Author(s) through the RFC process.*

## Motivation

### Immediate Need 
Users must have a standard way to give feedback and request help. There will be a general up-tick on support demand every time we release a new feature or roll out a new type of data. Having a basic ticketing system in place will help assure that no problems are missed. Understanding the needs of the DCP users is paramount. 

**This RFC should not be approved unless funding has been arranged**

### Support Sustainability 
Upon general availability (GA) of the DCP site to the public, the DCP team expects the rate of issue generation to increase. The current rate of issues is at about 1 per week. A burst of questions is expected upon first contact, which is what was experienced with the Preview site. The rate of tickets will drop to a steady flow after the initial burst, and will slowly increase as the number of active users of the system increases along with the data included and the general utility of the DCP increases.
Addressing the initial surge of tickets will require a big time commitment. Eventually we will need dedicated staff to handle support. An efficient ticketing triage system and a team well-versed in the DCP will be required to provide a good user experience of the DCP. 

This RFC describes an SOP that should satisfy both the Beta2 test phase and the early days of the product. As we grow in our understanding of user support needs, this SOP will likely need revision to address long-term sustainability.

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

## Definitions
`Knowledge Base` - a document that tracks all questions and answers that have been generated through the support infrastructure. This document can be mined to generate FAQs, or finding answers to obscure problems that have been solved in the past.
`Triager` - the first person to interact with an issue.

## **Detailed Design**

This section describes the flow of an issue through the support state
diagram presented below.

![](../images/0000-Zendesk-triage-SOP-diagram.jpeg)

## Issue Creation

Any user of the DCP can file an issue. Users can include data contributors, researchers trying to use the data in the DCP, and software developers trying to leverage the DCP API. 

While problems identified by an internal DCP staff member could be filed on Zendesk too, we favor a plan in which those issues are directly added to the applicable development repository, where they can be addressed directly. This plan eliminates the extra work for the triager of managing internally-generated tickets.

Once an issue is created in Zendesk there is a tiered support network that kicks in to resolve the question.

## Primary Support

The triage person, or triage team, is responsible for receiving initial support requests from end users, and monitors the Zendesk system daily for new tickets. Note that the Zendesk system automatically sends out an initial canned response to the user, indicating that the request was received and is being reviewed by our support staff. 

The Service Level Objective is to respond to the user with a solution no later than 48 business hours after it is filed.

The triager takes ownership of each open ticket and does the following to develop a response:

1. Evaluate whether there is enough information in the ticket to inform the response. If not, write back to the user and ask for the missing information. The user has 2 weeks to respond or the ticket will be closed.

2. Search the data portal documentation and FAQ for answers to the question. If the triage person can find an answer that resolves the problem, a response is sent to the user and the issue is closed. 

3. If an answer is not evident from searching the Knowledge Base in the data portal, the triage person needs to ask for help from the secondary support team. The secondary support team is comprised of at least one member from each DCP component or service. Communication with the team is facilitated via the `zendesk` slack channel on [humancellatlas.slack.com](humancellatlas.slack.com). It may be obvious which team should receive the ticket; if so, then it can be assigned directly. If it’s not obvious, there can be some internal discussion with the secondary support staff about who should receive the ticket.

4. At this point the ticket becomes the responsibility of the secondary support person, who should respond to the user and bring the ticket to an eventual close. The secondary support person MUST respond to the request within 2 days. 


The triager will also work with the secondary support staff to continuously improve the DCP site and make common solutions findable and available to the end user. 

1. They will tag each ticket with basic tags (data store/ingest/contribution/etc.) that help DCP staff categorize issues for later retrieval. In addition, Github tickets should be made for issues like: updating docs or writing new documentation and improving our FAQs, code examples, bug reports, etc. As much as possible, these solutions should be added to the Knowledge Base so that they can be turned into documentation or short FAQs and made available for users to access from the portal.

2. They will create and maintain canned responses (how to connect/get involved with the HCA; how to submit data, etc.) that can be quickly sent to users who have written with those questions. These canned responses will be stored in the Knowledge Base.

3. They will document discussions and decisions that arise from particular tickets in the Knowledge Base, so that it's clear who should respond to similar questions in the future and what the response should be.


## Secondary Support

The secondary support team is comprised of 1-2 members from each major component or service of the DCP. These representatives should have enough technical knowledge to answer most questions. They may require additional information from the person who created the ticket. They also may need to consult with technical experts who make up the tertiary support teams. Answers generated by the secondary support team will be added to the Knowledge Base by the secondary support team with the Triager's guidance.

## Tertiary Support

The Tertiary team is the entire DCP team. In the event that the secondary support team is not able to respond to the question, anyone may be called on to answer it. However, the need for Tertiary support should be rare, and all answers will be archived and added to the Knowledge Base

## **Unresolved Questions**
### **Funding**
Support can have a significant impact on development teams. Without funding development velocity will be much slower. Also using trained support staff will often result in a much better user experience.

We suggest that funding be sought to hire one full time or two part-time triage person(s) who would grow into the position. They should be able to handle primary triage early on as well as answer some of the more difficult questions, especially as the need for support increases over time. We will need a backup person for when the support person is on leave.

> 2 triage agents - could be up to 50% of their job as they would trade off
> being point for this job. Duties would include the grooming of the FAQ
> Knowledge Base. They would also be responsible for moderate our user
> forum. There is lots of work to be done to set up the support infrastructure.
> 
> 1 point person per service available to the end user - time required for support will vary
> depending on the problem. Each team MUST provide a contact person and commit to fixing 
> significant bugs in a timely manner. Dedicated personnel may be required as issue rate 
> approaches 1-2 tickets a day.


### **Prior Art**

Zendesk is already being used for our support mechanism. This RFC formalizes our processes.

### **Alternatives**


No other alternatives have been proposed.
