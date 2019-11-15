### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# Resource Based Access Control

## Summary

The DCP needs the ability to control access to projects across the different components in the DCP. This requires a 
centralized source of truth on who has access and an agreed process for managing access. This RFC seeks to unify the 
DCP’s storage and evaluation of project ACL using the described implementation.

## Author(s)

`[Trent Smith](mailto:trent.smith@chanzuckerberg.com)`

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

- Required to support [HCA Project Complete Stage RFC](https://github.com/HumanCellAtlas/dcp-community/blob/3ce2d4e786432d16a8540da599fb2f72a89f8c5c/rfcs/text/0015-project-completion-stage.md)
- Required to support Project Bases Access Control
- To limit the users view of the DCP to only the projects they have access.
- Allow storage of controlled access data that meets FISMA and GDPR access control requirement.

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

### Ingest
Ingest is where a project is first created and therefore is where ACL must first be applied to project. When a user 
creates a new project ingest will create a project resource in fusillade and assign the user as the owner. If a user 
wants to modify an existing project, ingest must first check with fusillade to see if the user has permission. Secondary
analysis will have permissions to submit a analysis bundle for a project on behalf of the owner.
 
### Data Store
Data store will consult fusillade before giving a user access to download project data. This will require modifying 
the get-file and get-bundle to check project association and access level before allowing
 access. Data store can use query service for this.

### Query Service
Query service will build an index associating project with bundles. This will be used in combination with fusillade 
to determine if a user has access to a bundle.

### Project Ownership

As the owner of a project you can assign ownership or remove ownership, as well as add data to the project, 
remove data from the project or delete the project. The owner can also make the project, "Project Complete" and visible to other users.

### Access levels
Several different access levels will be defined for a project. They are:
 - *Read* - a principal can read all data associated with this project
 - *ReadWrite* - a principal can read all data associated with this project and modify data associated with this 
 project. Writing data will involve minting new versions of the data, not removing data.
 - *Owner* - a principal can read, write, and share the project. Sharing grant other users access to the 
 project. Deleting, removes the project from the DCP.
 
### Hiding Project
Not all project should be visible to users. By default all new project are hidden and only viewable to the owner. The
 owner can make the project visible by make its access level public in fusillade.

### Acceptance Criteria [optional]

Affected components agree to implement the changes needed to support project based ACL.

*Acceptance criteria are the conditions that a RFC must satisfy to be accepted by users or other stakeholders.* 

### Unresolved Questions

- What actions can be performed on a project?
- Should owners be able to physically delete, or just tombstone?
- *What aspects of the design do you expect to clarify later during iterative development of this RFC?*

### Drawbacks and Limitations [optional]

*Why should this RFC **not** be implemented?*

### Prior Art [optional]

*Share references to prior art to deepen community understanding of the RFC, such as learnings, adaptations from earlier designs, or community standards.*

### Alternatives [optional]

*Highlight other possible approaches to delivering the value proposed in this RFC. 
What other designs were explored? What were their advantages? What was the rationale for rejecting alternatives?*

