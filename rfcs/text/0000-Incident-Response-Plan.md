### DCP PR:


# DCP Incident Response Plan

## Summary

DCP needs a clear process for handling incidents that may occur in the system and path for escalating incidents, as necessary. The proposed DCP Incident Response Plan (IRP) outlines the plan and procedures for these activities.

## Author(s)

Sarah Tahiri [stahiri@broadinstitute.org]

## Shepherd


## Motivation

Security incident response has become increasingly important as cybersecurity attacks have become more frequent, diverse, and damaging. Though preventive security measures can reduce the frequency of incidents, they cannot stop them all in their tracks. It is therefore necessary for DCP to have a documented Incident Response Plan that guides us in identifying, investigating, containing, mitigating, recovering from, and reporting an incident.

Additionally, we want the DCP system to be secure and reliable for all stakeholders, including engineering teams, program governance, and, above all, end users. In order to assure that we are following security best practices, and to help us attest to that fact in the future, we are working toward compliance with the NIST SP 800-53 Rev 4 security framework. Incident response is one of the requirements of this framework.

### User Stories

As a DCP technical team member, I want to know what constitutes an incident and what I should do if I see something suspicious actively occurring or find the results of malicious activity.

As the DCP security team, I want to be able to track all suspected or confirmed incidents in our system, know from investigation how the incident occurred and what it affected, and have reporting mechanisms in place so that we can notify the appropriate system stakeholders in a timely manner.

As DCP governance, I want be notified of a confirmed or suspected incident so that I may consider if there are any governance or legal ramifications. I also want to have confidence that the team had a process to follow so that the incident is identified, investigated, contained, and mitigated as quickly as possible.

## Detailed Design

Please review the draft [DCP Incident Response Plan](https://docs.google.com/document/d/1DW-WskpfEQHaFhnj4RFMAB5rf2W5OCpeUb19uXC2npI) which, at a high level, covers the following:
* Definitions of key terms (e.g., incident, breach)
* Incident response roles and responsibilities 
* Overview of the stages of incident response 
* Processes to follow in the event of a suspected or confirmed incident
* Defined incident escalation path

### Acceptance Criteria

The IRP must be confirmed as executable by all teams within the DCP community.

### Unresolved Questions

* Appendix A-1: Who are the Security Leads for each component? _Identifying these individuals is in progress._
* Appendix A-2: Is the table of key business stakeholders who should be informed of an incident complete?
* Appendix D: Is the escalation path complete and accurate? 

### Alternatives

Incident response is a key part of any security program and a requirement for compliance frameworks. No alternatives are available.
