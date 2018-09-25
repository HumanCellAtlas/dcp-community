### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`(dcp-community/rfc#)[https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>]`

# Metadata integration testing

## Summary

In order to stabilize the Data Coordination Platform (DCP) development environment, we propose implementing an automated continuous-integration procedure so that metadata schema changes are tested prior to being merged into the integration environment.

Importantly, if iterating on metadata schema in this new testing system is too slow, DCP project teams will have to iterate towards making their software more robust to metadata schema changes so that the process can be relaxed.

## Author(s)

* [Andrey Kislyuk](mailto:akislyuk@chanzuckerberg.com)
* [Sam Pierson](mailto:spierson@chanzuckerberg.com)
* [Matt Weiden](mailto:mweiden@chanzuckerberg.com)

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

Issues in the DCP software and metadata development process we need to solve

1. The DCP software development and integration environment is unstable, negatively impacting developer confidence and productivity
1. DCP does not practice "release early, release often"
    1. We have software and metadata freezes
    1. Updates are often not incremental; many changes are batched together, which has increased probability of system failure, introduced uncertainty, reduced productivity, and reduced developer confidence
1. Breaking changes are not sufficiently observable; the system is hard to debug
1. For metadata we “merge first, test later” - In failure cases, we merge metadata changes, then integration test fails; downstream projects adapt reactively; we can’t promote fixes to other problems
1. Systems that are not metadata-schema agnostic, do not fail early and predictably (should raise an error when a new major version of the schema that its developer hasn’t validated the software against before is encountered)
1. The agility of the metadata and ingest teams is hampered

To solve these issues we need to

1. Make the development environment more stable
1. Make updates to software and schema smaller and more frequent
1. Make failures observable
1. Make systems downstream of ingest more agnostic to schema or fail transparently

## Detailed Design

Below we describe a design that will transition DCP from the current "waterfall" software/metadata integration model to a more observable and interactive integration model, informed by integration tests that display the true compatibility state of the system.

Setting this plan will also better define the interaction between the metadata team and the rest of the DCP. Processes and responsibilities for resolving any integration issues that arise will be clear.

### Design Alternative #1

Software development proceeds in the dev branch of each component independently. Changes are merged into integration branch of each component only through PRs which cause system-wide integration tests to run. PRs are merged automatically if tests pass (but can be reverted if downstream breakage occurs). PRs may not be merged if tests do not pass.

![Option 1](../images/0000-metadata-integration-test-opt1.png)

*Here DSS Dev is shown as an example of other components following the same process.*

Data with the new schema version should not be uploaded into the `integration` or subsequent deployment stages until the test passes and the `master` branch is promoted to `integration`.

#### Pros

* "Test then merge"
* PR status will not require wranglers to know about downstream systems
* Small non-breaking changes can be merged quickly (out-of-order) and with confidence
* Greatly reduces probability of deployment stages past integration breaking
* More certainty for developers on behavior of the system
* Issues caused by metadata changes are surfaced early
* All DCP projects are integrated in the same way

#### Cons

* May be difficult to revert test versions of components and metadata (not all infra deployments are easily reversible)
* Requires "serializing" the dcp-wide integration test so that one PR can run after another

### Design Alternative #2

Software development proceeds in the dev branch of each component independently.
When a metadata schema version has passed unit tests in `dev` and its committers wish to promote it, they make a PR against their integration branch.
The PR triggers a dcp-wide test in the integration environment.
Metadata schema changes can be merged from `dev` to `integration` only if the integration test suite passes.

![Option 2](../images/0000-metadata-integration-test-opt2.png)

Data with the new schema version should not be uploaded into the `integration` or subsequent deployment stages until the test passes and the `master` branch is promoted to `integration`.

#### Pros

* "Test then merge"
* PR status will not require wranglers to know about downstream systems
* Small non-breaking changes can be merged quickly and with confidence
* Greatly reduces probability of deployment stages past `dev` breaking

#### Cons

* May introduce another deployment stage
* Treats metadata schema integration differently from other DCP projects

### Unresolved questions

* Will the metadata team be able to iterate on schema changes quickly enough with integration testing?
* Development plan for making DCP projects more robust to metadata schema changes
