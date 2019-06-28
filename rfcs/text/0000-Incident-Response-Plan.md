### DCP PR:


# DCP Incident Response Plan

## Summary

DCP needs a clear process for handling incidents that may occur in the system and path for escalating incidents, as necessary. The proposed DCP Incident Response Plan (IRP) outlines the plan and procedures for these activities.

## Author(s)

Sarah Tahiri [stahiri@broadinstitute.org]

## Shepherd


## Motivation

Security incident response has become increasingly important as cybersecurity attacks have become more frequent, diverse, and damaging. Though preventive security measures can reduce the frequency of incidents, they cannot stop them all in their tracks. It is therefore necessary for DCP to have a documented Incident Response Plan that guides us in identifying, investigating, containing, mitigating, recovering from, and reporting an incident.

Additionally, we want the DCP system to be secure and reliable for all stakeholders, including engineering teams, program governance, and, above all, end users. In order to assure that we are following security best practices, and to help us attest to that fact in the future, we are working toward compliance with the [NIST SP 800-53 Rev 4](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53r4.pdf) security framework. Incident response is one of the requirements of this framework.

### User Stories

As a DCP technical team member, I want to know what constitutes an incident and what I should do if I see something suspicious actively occurring or find the results of malicious activity.

As the DCP security team, I want to be able to track all suspected or confirmed incidents in our system, know from investigation how the incident occurred and what it affected, and have reporting mechanisms in place so that we can notify the appropriate system stakeholders in a timely manner.

As DCP governance, I want be notified of a confirmed or suspected incident so that I may consider if there are any governance or legal ramifications. I also want to have confidence that the team had a process to follow so that the incident is identified, investigated, contained, and mitigated as quickly as possible.

## Detailed Design

### Purpose
This Incident Response Plan (IRP) identifies and describes the Human Cell Atlas (HCA) Data Coordination Platform (DCP) process for investigating, tracking, and responding to cybersecurity incidents. It is in place to both manage and respond to any incidents or potential incidents that may occur. The goal of this plan is to detect and react to security incidents, determine their scope and risk, respond appropriately, communicate results and risks to stakeholders, and reduce the likelihood of the incident from reoccurring.

### Scope
This document establishes processes and procedures for implementing a security incident handling capability for the DCP system, the data contained within the system, and any person who gains access to the DCP system or data. 

### Definitions
* An __event__ is any observable occurrence in a system or network.
* An __adverse event__ is an event with a negative consequence, such as packet floods, unauthorized use of system privileges, unauthorized access to data, and execution of malware that destroys data.
* An __incident__ is a violation or imminent threat of violation of security policies, acceptable use policies, or standard security practices that actually or potentially jeopardizes the DCP’s ability to provide services or results in the exfiltration, change, deletion, or other compromise of data the system processes, stores, or transmits. An incident may occur as a result of malicious activity or internal error (e.g., operational or developmental errors).
* A __breach__ is any successful compromise at any level of protective controls to, or unauthorized access to or use of, the DCP system or data. Any attempt, whether successful or unsuccessful, is an incident, making a breach a subset of incidents.

### Roles and Responsibilities
The primary individuals and roles listed below play a part in the DCP incident reporting and response processes.
* __DCP Incident Response Team__: The DCP IRT serves as the focal point for IT security incidents for the DCP. The DCP IRT identifies security incidents, characterizes the nature and severity of incidents, and provides diagnostic and corrective actions when appropriate. The IRT will receive any reports of suspicious or confirmed incidents. The DCP IRT will be comprised of DCP Security Leads and security team members. 
* __Incident Response Team Coordinator__: The IRT Coordinator is a member of the IRT responsible for assembling all data pertinent to an incident, communicating with the appropriate parties, ensuring information is complete, overseeing the handling of the incident, and reporting on incident status both during and after investigation. This individual ensures the procedures outlined in this plan are effectively implemented. The IRT Coordinator is not intended to be a part of the solution team. Only the DCP Incident Response Team Coordinator is authorized to communicate directly with the HCA Oversight Committee for purposes of reporting an incident.
* __Incident Handlers__: Incident Handlers are members of the IRT who gather, preserve, and analyze evidence so an incident can be brought to a conclusion. Incident handlers are all DCP technical and security team members.
* __DCP Governance Group__: The Governing board of the DCP. It should be the first point of contact for the DCP Oversight Committee and developers, the HCA community for any DCP issues, and for funding organizations.
* __DCP Oversight Committee__: The Oversight Committee provides general oversight of the DCP projects and is tasked with escalating incidents, as necessary, to the DCP Governance Group. 
* __Project Lead__: Each DCP component is assigned a dedicated Project Lead who is responsible for clearly defining and communicating the long-term vision and direction of their project. Project Leads are informed of incidents and may be called upon, as necessary, to direct future enhancements to prevent an incident from recurring.
* __Security Lead__: Each DCP component has a Security Lead who is responsible for championing security initiatives for their project. Security Leads are also members of the IRT.

See _Appendix A: Incident Response Points of Contact_ for role assignments.

### Incident Severity Scale
In additional to tracking incident rate of occurrence, mean time to detect, and mean time to repair, DCP employs a standard rating of incident severity that describes the impact of the incident to the DCP.

The severity scale is determined by three general criteria:
* __Confidentiality__. Is the data clinical or non-clinical, open access or managed access?
* __Integrity__. Is the system producing correct results?
* __Availability__. Is the system up and running?

#### Incident Severity Ratings 
* __High__ - DCP is no longer able to provide some critical services to users; sensitive identifiable data was accessed without authorization or exfiltrated; or recovery from the incident is unpredictable or not possible.
* __Moderate__ - DCP has lost the ability to provide a critical service to a subset of users; non-sensitive data was accessed without authorization or exfiltrated; or recovery from the incident is predictable with additional resources.
* __Low__ - Minimal effect; DCP can still provide all critical services to users but has lost efficiency; recovery from the incident is predictable with existing resources.

### Incident Response Plan and Procedures
#### Incident Response Lifecycle Overview
The DCP IRT follows the incident response lifecycle described below.

![Incident Response Lifecycle](https://drive.google.com/uc?export=view&id=1cTXM31emPeS2RiaSI6p8yLfyZG8a6_R3)

##### Phase 1: Preparation
Preventing incidents from occurring is a part of preparation. DCP personnel implement appropriate security controls as part of the overall DevSecOps program. Preparation activities related to incident response are as follows:
* Establish an incident response capability
* Develop incident response policies and procedures

##### Phase 2: Detection and Analysis
Incident identification occurs in this phase. The DCP IRT learns of an actual or suspected security incident. Incidents may be detected through many different means with varying levels of detail and veracity. Security incidents may be detected through some of the following examples:
* End user identification
* System administrator identification
* Log review
* Automated alerting tools
* Locked out accounts
* Pop-ups and redirected websites when browsing 
* Unexpected software installs
* Unexplained changes to files
* Anomalies in normal network traffic patterns
* Abnormal outbound traffic
* Irregular access locations
* Large number of requests for the same object or files
* Suspicious activity on the network after-hours
* Multiple failed login attempts
* Unknown/unauthorized IP addresses
* Services and applications configured to launch automatically

If there is confirmation of an incident, the IRT formulates an overall response strategy and reports the incident to the Oversight Committee and other concerned parties, as appropriate.

##### Phase 3: Containment, Eradication, and Recovery
This phase is used to gather electronic and physical evidence to determine exactly what has happened, who might be responsible, and how this type of intrusion can be prevented in the future.  The DCP IRT will make recommendations to remediate affected resources and verify completion. As necessary, the IRT will take measures to limit the amount of damage that an intruder or authorized affected system can inflict upon the rest of the IT infrastructure. 

##### Phase 4: Post-Incident Activity
Follow-up occurs in this phase. DCP IRT completes a report of the incident, subsequent recovery process, and lessons learned. A final copy of the incident report is sent to the Oversight Committee and appropriate stakeholders. Additionally, an incident post-mortem may be conducted to evaluate the lessons learned during the event.

#### Incident Response Procedures
Anyone can declare an incident. Follow the procedure outlined below if you suspect or have confirmed that an incident has occurred.

__Step 1__: Personnel identifying the suspected or confirmed incident (Incident Handler) will alert the DCP IRT by submitting a ticket to the to-be-created humancellatlas/dcp-security private GitHub repo. The ticket will include the following information to ensure appropriate triage:
1. Assign the ticket to the DCP IRT Coordinator
1. Date of discovery of the suspected or confirmed incident
1. Brief description of suspected or confirmed incident
1. Estimated incident severity rating
1. If known, also include:
   1. Date incident occurred
   1. Date incident was discovered
   1. Affected system(s) and application(s), with associated version information
   1. Number of users affected
   1. Data types involved

__Step 2__: Incident Handler will post an incident notification to the HumanCellAtlas/dcp-security Slack channel. The notification should not contain detailed information; instead, it should link to the incident’s tracking ticket. 

Example: A suspected or confirmed incident has been identified and is being tracked in [insert_ticket_link_here].

__Step 3__: DCP IRT will immediately review the submitted ticket and, with assistance from Incident Handlers, conduct a preliminary investigation of the event to determine if the event constitutes a security incident.
* __Step 3.1__: If the DCP IRT determines that the event does not constitute a security incident or is a false positive, the DCP IRT Coordinator will document the determination and supporting evidence within the ticket and close the ticket. No further action is required.
* __Step 3.2__: If the DCP IRT confirms that the event does constitute a security incident, the DCP IRT will verify or update the incident severity rating and the DCP IRT Coordinator will collaborate with the Incident Handler to save all evidence and complete an Incident Report (see Appendix B).

__Step 4__: The DCP IRT Coordinator will update the incident’s ticket to include a link to the completed Incident Report and notify the following that the report has been completed:
* HumanCellAtlas/dcp-security Slack channel 
* DCP Business Stakeholders
* Oversight Committee

__Step 5__: The DCP IRT Coordinator will work with the Oversight Committee regarding escalation steps, as necessary, until Oversight confirms that the incident has been satisfactorily resolved. 

__Step 6__: The DCP IRT Coordinator will update the Incident Report to formally document resolution of the incident and notify the HumanCellAtlas/dcp-security Slack channel.

__Step 7__: The DCP IRT Coordinator will prompt appropriate DCP team members to participate in an incident review and lessons learned meeting within two business days of issue resolution. Questions to be answered during the lessons learned includes:
* Exactly what happened and when?
* How did staff and management perform dealing with the incident? Were the documented procedures followed? Were they adequate?
* What information was needed sooner?
* Where any steps taken that might have inhibited the recovery?
* What would staff and management do differently the next time a similar incident occurred?
* What corrective actions can prevent similar incidents in the future?
* What precursors or indicators should be watched for in the future to detect similar incidents?
* What additional tools or resources are needed to detect, analyze and mitigate future incidents?

Information gathered in an incident’s lessons learned will be captured in an Incident Review Report (see Appendix C), which can be used in the future as a reference for handling similar incidents. 

__Step 8__: The DCP IRT Coordinator or designee will link the final Incident Review Report to the incident’s GitHub issue and close the ticket.

### Appendix A. Incident Response Points of Contact
#### Appendix A-1. DCP Incident Response Team
Incident Response Role|Name|Title|Email
----------------------|----|-----|-----
IRT Coordinator|Sarah Tahiri|Sr. Security Program Manager, Broad Institute|stahiri@broadinstitute.org
Alternate IRT Coordinator|Rhian Anthony|DevOps Engineer, Broad Institute|ranthony@broadinstitute.org
Security Lead, Data Portal|Dave Rogers|Contract Engineer - Clever Canary, UCSC|dave@clevercanary.com
Security Lead, Data Browser|Hannes Schmidt|Technical Lead, UCSC|hannes@ucsc.edu
Security Lead, Data Storage Service (DSS)|Trent Smith|Engineer, CZI|trent.smith@chanzuckerberg.com
Security Lead, Ingest Support|||
Security Lead, Ingestion Service|Rodrey Goite|Senior Software Developer, EBI|rodrey@ebi.ac.uk
Security Lead, Metadata Schema|||
Security Lead, Expression Matrix Service|Calvin Nhieu|Engineer, CZI|cnhieu@chanzuckerberg.com
Security Lead, Upload Service|Sam Pierson|Engineer, CZI|spierson@chanzuckerberg.com
Security Lead, Query Service|Andrey Kislyuk|Engineer, CZI|akislyuk@chanzuckerberg.com
Security Lead, Pipeline Portability|Rhian Anthony|DevOps Engineer, Broad Institute|ranthony@broadinstitute.org
Security Lead, Data Processing Pipelines|Phil Shapiro|Engineering Manager, Broad Institute|pshapiro@broadinstitute.org
Security Lead, Pipeline Execution Service|Rhian Anthony|DevOps Engineer, Broad Institute|ranthony@broadinstitute.org
Security Lead, Unity Benchmarking Service|Rhian Anthony|DevOps Engineer, Broad Institute|ranthony@broadinstitute.org

NOTE: Security Leads for each component are identified in the [DCP Roster](https://docs.google.com/spreadsheets/d/1f0RGNrEYsIOYYg9TRKK0dJgq_MDVKnrAnzPRvQqGLec/edit#gid=656341491).

#### Appendix A-2. DCP Incident Response Business Stakeholders
Name|Role|Title|Email|Description
----|----|-----|-----|-----------
DCP Project Leads|Project Lead|N/A|project-leads@data-humancellatlas.org|Project Lead for each DCP component

### Appendix B. Incident Report Template
https://docs.google.com/document/d/1FADJEms3Dll4nqDvutzENXrAS7QURPVEQrhcB1oJRaA/edit?usp=sharing

### Appendix C. Incident Review Report
https://docs.google.com/presentation/d/1ijKMyip1pOFdro1rbbl3AsPvh3VW79lKHxj7bX-NTjY/edit#slide=id.g557a122a6c_0_0

## Acceptance Criteria

An incident is correctly managed by the process described herein. If an incident cannot be fully managed by this process, the IRP will be updated as required for execution.

## Unresolved Questions

* Appendix A-1: Who are the Security Leads for each component? _Identifying these individuals is in progress._
* Appendix A-2: Is the table of key business stakeholders who should be informed of an incident complete?
* Appendix D: Is the escalation path complete and accurate? 

## Alternatives

Incident response is a key part of any security program and a requirement for compliance frameworks. No alternatives are available.
