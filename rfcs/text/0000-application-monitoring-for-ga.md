### DCP PR:

# System monitoring for General Availability

## Summary

This RFC describes the first standard revision for monitoring the distributed systems of the Human Cell Atlas (HCA) Data Coordination Platform (DCP). The standard is scoped to the minimum viable set of monitoring infrastructure the DCP will need to reach General Availability (GA). We anticipate that future revisions will be needed to meet our Service Level Objectives (SLOs) as DCP matures.

The term monitoring is used here as defined in the [Google Site Reliability Engineering (SRE)](https://landing.google.com/sre/sre-book/chapters/monitoring-distributed-systems/) program. This document borrows heavily from SRE.

## Author(s)

* [Matthew Weiden](mailto:mweiden@chanzuckerberg.com)

## Shepherd


## Motivation

Monitoring the distributed systems [enables operators](https://landing.google.com/sre/sre-book/chapters/monitoring-distributed-systems/#why-monitor-pWsBTZ) to analyze long-term trends in system performance, compare experiments against a baseline, alert on failures, and conduct retrospective analysis on systems (debugging). These abilities are critical to keeping software correct and available.

### User Stories

* As a DCP operator, I want to compare experiments against a baseline, so that I can compare design alternatives.
* As a DCP operator, I want to conduct retropsective analysis on systems, so that I can debug systems.
* As a DCP operator, I want to analyze long-term trends in system performance, so that I can estimate changes in cost and performance.
* As a DCP operator, I want to be alerted of system failure, so that I know when something goes wrong and can keep the system available for our users.

## Detailed Design

In summary, each user-critical system in DCP should have the following.

* Black-box availability monitoring using a health check
* White-box monitoring of core system infrastructure
* Alerts on black-box health check failures and white-box infrastructure anomalies
* A dashboard that summarizes your white-box metrics

The following subsections recommend concrete implementations of each requirement.

### Overview of monitoring tooling

Currently, the following tooling is in place to help DCP operators monitor their systems. Further, it indicates whether use of the tool should be adopted as a best practice within DCP as part of this proposal.

| Purpose | Tool/framework | Deployments | Roadmap |
|------|---------|-------------|--------------|
| Centralized configuration for health checks, custom metrics, alerts, and dashboards | [dcp-monitoring](https://github.com/HumanCellAtlas/dcp-monitoring) | [github+terraform](https://github.com/HumanCellAtlas/dcp-monitoring) | adopt as best practice |
| Visualization layer that can plot metrics from any datasource | Grafana | [dev/integration/staging](https://metrics.dev.data.humancellatlas.org/), [prod](https://metrics.data.humancellatlas.org/) | adopt as best practice |
| Metrics for AWS applications and managed services | [CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric) | [console](https://console.aws.amazon.com/cloudwatch/home?region=us-east-1#metricsV2:) | adopt as best practice for AWS |
| Metrics for GCP applications and managed services | [Stackdriver Metrics](https://cloud.google.com/monitoring/api/metrics) | [console](https://console.cloud.google.com/monitoring) | adopt as best practice for GCP |
| Alerting based on CloudWatch Metrics | [CloudWatch Alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#CloudWatchAlarms) | [github+terraform](https://github.com/HumanCellAtlas/dcp-monitoring) | adopt as best practice |
| Metrics based on operational log indices | [Elasticsearch Metrics](http://docs.grafana.org/features/datasources/elasticsearch/) | [dev/integration/staging](https://logs.dev.data.humancellatlas.org/), [prod](https://logs.data.humancellatlas.org/) | use not recommended, but sometimes necessary |
| Build statuses, system health, and availability reporting | [Status API](https://github.com/HumanCellAtlas/status-api) | [dev/integration/staging](https://status.dev.data.humancellatlas.org/), [prod](https://status.data.humancellatlas.org/) | use optional |
| Public presentation of build statuses, system health, and availability reporting | [DCP Status Page](https://github.com/HumanCellAtlas/humancellatlas.github.io) | [https://humancellatlas.github.io/](https://humancellatlas.github.io/) | temporary until formal status page is built

### Black-box monitoring

Black-box monitoring attempts to test system behavior as a user would see it.

To estimate and check this behavior, implement an HTTP health check endpoint for your service, a [Route53 health check](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/route-53-concepts.html#route-53-concepts-health-check) in the [dcp-monitoring](https://github.com/HumanCellAtlas/dcp-monitoring) repository.

Your system's health check should aggregate the health status of its API and any infrastructure it depends on. For example, if your service is a HTTP REST API that holds state in a database, the health check endpoint would return a healthy status if the API was working and a basic query, such as `SELECT 1`, could be performed against the database and an unhealthy status otherwise.

### White-box monitoring

In order to diagnose problems, critical application logic and infrastructure that each system depends on must be monitored. To illustrate, consider an API that depends on AWS S3 and Elasticsearch. If the API goes down and its logs are not informative, there is no way of telling what infrastructure component failed unless there is separate monitoring of each component.

 Use [CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric), [Stackdriver Metrics](https://cloud.google.com/monitoring/api/metrics), or log-based metrics ([AWS](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CloudWatchLogsConcepts.html), [GCP](https://cloud.google.com/logging/docs/logs-based-metrics/), [Elasticsearch](http://docs.grafana.org/features/datasources/elasticsearch/)) to white-box monitor your critical application logic and infrastructure. 
 
 If you do not know what to monitor in your application, review the [four golden signals](https://landing.google.com/sre/sre-book/chapters/monitoring-distributed-systems/#xref_monitoring_golden-signals). Many of these metrics are provided out-of-the-box by AWS and GCP managed services.
 
 Your monitoring coverage should be as "[simple as possible, no simpler](https://landing.google.com/sre/sre-book/chapters/monitoring-distributed-systems/#as-simple-as-possible-no-simpler-lqskHx)" to diagnose common failure modes of your system. Some common examples are included below.
 
 * Common failure modes of Elasticsearch clusters are related to JVM memory pressure and minimum shard storage space. These are good things to have on a dashboard.
 * HTTP APIs should have a dashboard summarizing rates of requests by HTTP status class (`2xx`, `3xx`, `4xx`, `5xx`) and request latencies by percentile (commonly 50th, 75th, 95th, 99th). Adding an optional route and method dimensions to each metric will help you pinpoint what is failing when errors occur.
 * Queues should be monitored for length, size (bytes), consumption rates by status, and age of oldest message.
 
### Alerts

We need alerts (or alarms) in order to be notified of problems in the system.

Configure alerts in the [dcp-monitoring](https://github.com/HumanCellAtlas/dcp-monitoring) repo with a [CloudWatch Alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#CloudWatchAlarms). All black-box Route 53 health checks should be configured with an alert. Optionally, you may choose to set thresholds on some of your white-box monitoring metrics if they are highly-correlated with system failure (e.g. an EC2 node running out of disk space).

Alerts are currently propagated to the Slack channel specified in the CloudWatch Alarm configuration.

### Dashboards

Dashboards answer basic, frequently-asked questions about your service. Ideally, they should be a high-level, easy-to-read summary of the metrics from a system's white-box monitoring.

Create and host dashboards for your service using our [Grafana deployments](https://github.com/HumanCellAtlas/metrics). You can template your dashboards to generalize to multiple deployments of your software using [dcp-monitoring](https://github.com/HumanCellAtlas/dcp-monitoring).

![](../images/0000-dashboard.png)
*An example dashboard visualizing white-box metrics for an HTTP API backed by an Elasticsearch cluster to store state. The performance of the HTTP API is monitored in the first row with HTTP request rates, statuses, and latencies. The second row monitors the metrics that would predict system failure due to Elasticsearch cluster overload.*

### Status and availability monitoring

System status and availability monitoring is provided by a [Status API](https://github.com/HumanCellAtlas/status-api), which then can be summarized on a [status page](https://humancellatlas.github.io/).

### Unresolved Questions

* The specific lists of white-box infrastructure to be monitored for each component
* Alerting strategies other than Slack notifications

### Drawbacks and Limitations

* Data sources for our monitoring system are extremely heterogeneous. We have AWS CloudWatch Metrics, StackDriver Metrics, Elasticsearch, and a log-based metrics system for each cloud platform. 

### Alternatives

* Have no centralized monitoring tooling; leave it up to teams as to how they want to monitor their applications.
* Due to the heterogeneity of data sources in DCP, there is a strong argument to use metric exporters to export all metric data to a single metrics server. There would then be true, global metric aggregation and a single metric query language.

