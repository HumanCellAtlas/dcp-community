### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# RFC Name

Metadata schema usability

## Summary

*Describe the vision of the RFC in 2-3 sentences. Consider this section as your "elevator pitch" or "release note" to the community.*

An approach to generating a user-friendly version of the metadata JSON schema from the modularized source is proposed.  These *demodularized* schemas will be built from the current source and be the standard user view of the data modle.   This addresses issues with the schema structure being difficult to understand and with field names and descriptions defined in modules not reflecting the user-visible instance data type.

## Author(s)

*Recommended format for Authors:*

 `[Mark Diekhans](mailto:markd@ucsc.edu)`
 `[Simon Jupp](mailto:jupp@ebi.ac.uk)`
 `[Mallory Freeberg>](mailto:mfreeberg@ebi.ac.uk)`
 `[Dani Welter](mailto:dwelter@ebi.ac.uk)`
 `[Tony Burdett](mailto:tburdett@ebi.ac.uk)`

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*
 
 `[Name](mailto:username@example.com)`

## Motivation

*Describe the user or technical need in detail [with alignment to the DCP roadmap priorities where possible]. Link prior community discussions to demonstrate support for this RFC.*

Consumers of the HCA metadata JSON Schema have found that the module structure makes the schema difficult to understand, even for experienced programmers. Currently, To support 26 different instance types, there are 78 JSON Schema files.  While not an overly large number of modules, the composition model of JSON schema does not match any standard type paradigm, making it difficult to explain.  With no tools available for visualizing or understanding JSON Schema, this makes the HCA data model opaque.

Another result of the module structure is that field names and descriptions defined in modules don't match the concrete class.  This makes schema-driven users interfaces confusing.

However, the module structure adds value in making the schema easier to create, maintain, and keep consistent.


### User Stories

*Share the [User Stories](https://www.mountaingoatsoftware.com/agile/user-stories) motivating this RFC.*

* 

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

