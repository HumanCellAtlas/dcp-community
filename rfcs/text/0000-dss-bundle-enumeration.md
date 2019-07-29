### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# RFC: Bundle Enumeration of the DCP Data Store (DSS)

## Summary

This RFC proposes bundle enumeration endpoint(s) for the DSS.

## Author(s)

* [Brian Hannafious](mailto:bhannafi@ucsc.edu)

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

Currently, the only means of listing bundles stored in the DSS is the internal Elasticsearch (ES) metadata index, which
must be kept current with object storage. The DSS should provide bundle enumeration independently of the ES metadata index,
emphasizing consistency and scalability.

### User Stories

* As a downstream service developer, I would like to enumerate the bundle contents of the DSS so I can create my own
  index.

* As a downstream service developer, I would like to check if my index contains all the bundles in the DSS.

## Detailed Design

A new bundle enumeration endpoint, `GET /bundles`, will be introduced, taking replica and prefix parameters. These
parameters will be used to return a paginated listing of bundles directly from object storage. Pagination semantics
and all other semantics of this route will be in line with the established conventions of the DSS API.

### Unresolved Questions
