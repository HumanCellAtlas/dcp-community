### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# RFC Name

IaaS Resource Tagging for and Cost Monitoring and Auditing

## Summary

Define mandatory tagging and auditability requirements for DCP cloud (IaaS) resources and operator identities, to be used for:

- Cost accounting, analysis, and monitoring;
- Security auditing of administrative and operator actions.

## Author(s)

* [Amar Jandu](mailto:ajandu@ucsc.edu)
* [Andrey Kislyuk](mailto:akisilyuk@chanzuckerberg.com)

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

A substantial component of the HCA DCP budget is IaaS (cloud infrastructure) cost (specifically, AWS and GCP costs). We
would like to monitor the sources of these costs on a service and component level. The separation of development and
production IaaS accounts naturally allows us to track the cost breakdown on that dimension. AWS and GCP also provide
a way to track costs by underlying IaaS service (such as S3 vs. EC2). However, we have no comprehensive way to separate
costs by DCP service (such as Ingest Service vs. Data Store), development environment (dev vs. staging), or service component
(such as DSS sync daemon vs. DSS checkout daemon).

Monitoring costs by service and component will allow us to track down cost drivers among our components, prioritize
engineering efforts to optimize our use of underlying cloud infrastructure, empower our developers to get daily feedback about
the consequences of their serivce architecture design decisions, identify cost anomalies indicating possible abuse or security
issues, and provide cost reporting, forecasting, and accountability to our funders.

### User Stories

* As a DCP developer, I would like to see how much a particular component of my service is costing in a particular test
  environment, such as a routine scale test.
* As a DCP operator, I would like to keep an eye on the running costs incurred by DCP services, to identify unexpected
  cost increases or service availability issues.
* As a DCP security lead, I would like to identify resource provisioning actions performed in the production account and
  track them by operator identity and service.

## Detailed Design

We will use
[AWS cost allocation tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/custom-tags.html#allocation-how)
and [GCP labels](https://cloud.google.com/resource-manager/docs/creating-managing-labels) to tag all resources that can
be tagged. For resources that support tagging/labeling and constitute a significant portion of the IaaS budget (over 1%),
tagging will be required. For other resources that support tagging/labeling, it is encouraged, but not required.

#### AWS Services that Require Resource Tags

AWS services that specifically require tagging because they have been previously identified as cost drivers are:

* AWS Lambda
* API Gateway
* S3
* EC2 (including all subcomponents that are cost drivers)
* RDS
* Elasticsearch Service
* ELB
* ECS
* DynamoDB
 
#### Standard Cost Control Tag/Label Keys

The following tags (AWS) or labels (GCP) **MUST** be set on all HCA DCP assets for which the following conditions apply:

* Their underlying services support tagging/labeling
* Their underlying services amount to 1% or greater cumulative IaaS spend in the HCA DCP budget in the prior calendar month

   **NOTE** : keys and values are case-sensitive

Name        | Description
------------|-------------------------------
`project`   | The value for the project key shall be `dcp`
`env`       | The value for the env key shall be the stage that the resource applies to, this can be `{dev,staging,integration,prod}`
`owner`     | The email of a team or individual responsible for operating the asset, for example: dss-team@data.humancellatlas.org
`service`   | The name of the HCA DCP service that manages the resource, for example: DSS, IngestService
`Name`      | A human-readable name for the asset. This can be a custom name or a combination of other tags, such as `dss-notify-dev` (service-subcomponent-env). The name should identify the service subcomponent that the asset belongs to, if any. (The value of this tag shows up as the asset name in the AWS Console UI.)
`managedBy` | The name of a deployment management tool managing this asset, for example: `terraform`, `cloudformation`, `chalice`


#### Termination policy
Assets that are not tagged in accordance with the above are subject to termination.

* After this RFC takes effect, a 90 day grace period will apply, in which all DCP services are expected to implement tagging.
* After the grace period, an automated "reaper" process will periodically flag untagged assets and announce this in the
  `#dcp-ops-alerts` channel on the HCA Slack.
* Any resources mentioned in such an announcement that cost over $100/day will be subject to immediate termination.
* Any other resources mentioned in such an announcement will be terminated after a 7 day grace period elapses.

### Role session name (AWS only)
To facilitate auditing of operator actions, all operators **MUST** configure a `role_session_name` parameter in their AWS CLI
configuration.

* See the
  [HCA DCP Ops Wiki](https://allspark.dev.data.humancellatlas.org/dcp-ops/docs/wikis/Setting%20up%20the%20AWS%20Console%20and%20AWS%20CLI%20to%20work%20with%20HCA%20DCP)
  for more information regarding `role_session_name`.
* The [AWS-managed tag `aws:createdBy`](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/aws-tags.html)
  incorporates the resulting principal name, and can be used to track resources in AWS auditing tools, including the
  Cost Explorer.
* After this RFC takes effect, a 90 day grace period will apply, in which all DCP developers and operators are expected to
  configure AWS role session names for their accounts.
* After the grace period, access may be denied when executing AssumeRole without an identifiable session name.

#### Example: Tagging with Terraform
The following is an example of a default set of tags that can be applied to an AWS Resource in Terraform.
Using this local.common_tags allows these tags to be defined in one section for the variables, and be used multiple times.
If a resource requires an additional independent tag, this can also be achieved.

```
// variables.tf
locals {
  common_tags = "${map(
    "managedBy" , "terraform",
    "Name"      , "${var.project}-${var.env}-${var.service}",
    "project"   , "${var.project}", // dcp
    "env"       , "${var.env}" // stage {dev,integration,staging,prod}
    "service"   , "${var.service}", // dss 
    "owner"     , "${var.owner}" // Team/Memeber contact email
  )}"   
}
```

```
// only the default tags AWS
resource aws_s3_bucket dss_s3_bucket_test_fixtures {
  count = "${var.DSS_DEPLOYMENT_STAGE == "dev" ? 1 : 0}"
  bucket = "${var.DSS_S3_BUCKET_TEST_FIXTURES}"
  tags = "${local.common_tags}"
}
```

```
// only the default tags GCP
resource google_storage_bucket dss_gs_bucket {
  name          = "${var.DSS_GS_BUCKET}"
  provider      = "google"
  location      = "US"
  storage_class = "MULTI_REGIONAL"
  labels = "${local.common_tags}"
}
```


```
// default tags and resource-specific tags

resource aws_elasticsearch_domain elasticsearch {
    tags = "${merge(
        local.common_tags,
        map(
           "Domain", "${var.DSS_ES_DOMAIN}"
        )
    )}"
}
```

Once a tag set for a resource has been established it should be updated within the [DCP Resource Tagging wiki](https://allspark.dev.data.humancellatlas.org/dcp-ops/docs/wikis/Resource%20Tagging) so possible configurations can be seen.

#### Example: Querying the AWS Resource Group Tagging API
The following script showcases how consistent resource tagging can be leveraged for automatic reporting.
```bash
#!/bin/bash
  
set -euo pipefail

service=$1
resource_type=$2

aws resourcegroupstaggingapi get-resources --tag-filters Key=service,Values=${service} \
    | jq --arg parn "arn:aws:${resource_type}" '.ResourceTagMappingList[] | select(.ResourceARN | contains($parn))' \
    | jq -r .ResourceARN
```
Example invocation of the script: `find_resources.sh dss s3`

### Acceptance Criteria

A set of tags/labels is defined and required for Cloud resources to have.

A role name policy is defined and required for DCP operators and developers to use.

### Unresolved Questions

Are there other tags that we need to have?

Are there other resources that should have tags become required for them?

### Alternatives

Services have independent tags that are posted within the Resource Tagging wiki, and admins have to track them down.
