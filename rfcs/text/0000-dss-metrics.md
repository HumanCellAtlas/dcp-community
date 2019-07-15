### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# RFC: Key performance metrics of the DCP Data Store (DSS)

## Summary

This RFC proposes a set of key performance metrics to be introduced and tracked as a summary measure of performance by
which the utility of the Data Store can be evaluated, including improvements and regressions.

## Author(s)

* [Andrey Kislyuk](mailto:akislyuk@chanzuckerberg.com)

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

Beyond latency and error rates, the DSS lacks a clear focus on quantifiable assessment of usability. We would like to
quantify higher level performance metrics that reflect the Data Store's ability to make HCA data accessible, computable,
and durable.

### User Stories

* As a DSS developer, I would like to know whether the DSS is improving in its ability to serve downstream users and
  services.

* As a downstream service developer, I would like to suggest a way of measuring DSS performance that reflects my ability
  to use the DSS to consume HCA data.

* As a DCP funder, I would like to know how the DSS is balancing the goals of economical data storage and data
  accessibility and computability.

## Detailed Design

Introduce infrastructure (such as logging, instrumentation, offline log analytics, etc.) to measure the following DSS
performance metrics:

- Measure TTFB (time to first byte) along with any dimensions that affect checkout service behavior (file type, size,
  etc.), User Agent string, and other relevant request data. Time to first byte is defined here as the time from the
  first request to access a bundle or file to the time of the first request that returns a pre-signed or direct object
  storage access URL. (This may require an opaque session identifier to be returned when serving responses with a
  Retry-After header.)

- Measure unrecoverable error rate, i.e. the rate of error that caused the client to stop trying and cause a client-side
  error to be raised. (This may require instrumentation in the HCA CLI.)

- Measure "all-in" cost per GB stored (the total cost of DSS services provided divided by the number of GB stored in
  each replica). (Other metrics or breakout categories could be needed here, such as breaking out and tracking transfer
  or subscription notification costs separately, but only if and when it is shown that such costs are a significant
  component of the overall cost footprint.)

Measure the above metrics on both AWS and GCP, separately and in composite.

A blend of these metrics can be thought of as the key indicator for the Data Store to optimize.

### Unresolved Questions
