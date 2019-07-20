### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# RFC Name

Data processing robustness

## Summary

*Describe the vision of the RFC in 2-3 sentences. Consider this section as your "elevator pitch" or "release note" to the community.*

The HCA DCP is a data-driven system where the input experimental comes from external sources that might provide incorrect or corrupt data.  While data ingested into the system is validated, this process is not perfect and may miss problems.  Metadata is part of the data, it will change as different types of experiments are consumed.  This may happen in ways that differed from the assumptions of software processing the data.

As part of normal operations, all components in the system are responsible for:
1. Skipping experimental data when it is erroneous or unexpected.
1. Capturing failures in a way the they can be detected and corrected.
1. Reprocess failed data when the process are corrected.
1. Ensure consistency of all output data.

## Author(s)

[Mark Diekhans](mailto:markd@ucsc.edu),
[Tony Burdett](mailto:tburdett@ebi.ac.uk)

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

*Describe the user or technical need in detail [with alignment to the DCP roadmap priorities where possible]. Link prior community discussions to demonstrate support for this RFC.*

### User Stories

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

