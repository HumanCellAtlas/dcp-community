### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# Resource Based Access Control

## Summary

The HCA needs the ability to control access to specific resources in the DCP such as a project. Additional API endpoints
 are proposed for the Authorization Service(fusillade) to fulfill these requirements. This will provide fine grained 
 access control for all resources in the DCP. 

## Author(s)

`[Trent Smith](mailto:trent.smith@chanzuckerberg.com)`

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

- Required to support [HCA Project Complete Stage RFC](https://github.com/HumanCellAtlas/dcp-community/blob/3ce2d4e786432d16a8540da599fb2f72a89f8c5c/rfcs/text/0015-project-completion-stage.md)
- Required to support Project Bases Access Control
- To limit the users view of the DCP to only the resource they have access.
- Allow storage of controlled access data that meets FISMA and GDPR access control requirement.


*Describe the user or technical need in detail [with alignment to the DCP roadmap priorities where possible]. Link prior community discussions to demonstrate support for this RFC.*

### User Stories

1. See user stories in [HCA Project Complete Stage RFC](https://github
.com/HumanCellAtlas/dcp-community/blob/3ce2d4e786432d16a8540da599fb2f72a89f8c5c/rfcs/text/0015-project-completion-stage.md).

1. As a data submitter, I need to limit access to my project, so that I can't ensure it is version-complete before 
 making public.

1. As a data submitter, I would like to give my lab access to my project, so they can assist in validating secondary 
 analysis results.

1. As a data submitter, I would like to make my private project accessible to the public, so that other user of the DCP
 can benefit.  

1. As a data submitter, I would like to submit data that has personally identifiable information (PII) and therefore 
 need special permissions to access.
 
1. As a component of the DCP, I would like to limit a user's view to only resource they have permission to view, to 
 prevent exposing privileged information to a users.

1. As an operator of the DCP, I would like to know why a user has access to a resource, so I may reason about that 
 whether a user is over privileged. This is helpful for maintaining a access policy of least privileges.

*Share the [User Stories](https://www.mountaingoatsoftware.com/agile/user-stories) motivating this RFC.*

## Scientific "guardrails" [optional]

*Describe recommended or mandated review from HCA Science governance to ensure that the RFC addresses the needs of the scientific community.*

## Detailed Design

*Explain the design in sufficient detail such that the implementation and (if appropriate) the interaction with existing DCP software are both reasonably clear.*

### Acceptance Criteria [optional]

*Acceptance criteria are the conditions that a RFC must satisfy to be accepted by users or other stakeholders.* 

### Unresolved Questions

- *What aspects of the design do you expect to clarify further through the RFC review process?*
- *What aspects of the design do you expect to clarify later during iterative development of this RFC?*

### Drawbacks and Limitations [optional]

*Why should this RFC **not** be implemented?*

### Prior Art [optional]

*Share references to prior art to deepen community understanding of the RFC, such as learnings, adaptations from earlier designs, or community standards.*

### Alternatives [optional]

*Highlight other possible approaches to delivering the value proposed in this RFC. 
What other designs were explored? What were their advantages? What was the rationale for rejecting alternatives?*

