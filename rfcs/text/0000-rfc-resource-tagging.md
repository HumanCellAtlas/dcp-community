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
be tagged. Resources that support tagging/labeling and constitute a significant portion of the IaaS budget (over 1%), tagging
will be required. For other resources that support tagging/labeling, it is encouraged, but not required.

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

## Proposed Tag Keys:
   **NOTE** : keys and values are case-sensitive

### project:
    The value for the project key shall be `dcp`

### env:
    The value for the env key shall be the stage that the resource applies to, this can be;
    `{dev,staging,integration,prod}`

### owner:
    The value for the owner key shall be the email address attached from the role_session_name that was defined within the aws config file. Please note that this also applies to IAM users/configureations that any Coninuous Deployment is also using. See the [wiki](https://allspark.dev.data.humancellatlas.org/dcp-ops/docs/wikis/Setting%20up%20the%20AWS%20Console%20and%20AWS%20CLI%20to%20work%20with%20HCA%20DCP) for information regarding the role_session_name.

### service:
    The value for the service key shall be the service that manages the resource.

## name:
   the value for the name key shall be the values for the keys `project,env,service` separated by `-`. 

## Tagging with Terraform:
The following is an example of a defualt set of tags that can be applied to an AWS Resource in Terraform.
Using this local.common_tags allows these tags to be defined in one section for the variables, and be used multiple times, if a resource requires and additional intependent tag, this can also be achieved.
```
// variables.tf
locals {
  common_tags = "${map(
    "managedBy" , "terraform",
    "Name"      , "${var.project}-${var.env}-${var.service}",
    "project"   , "${var.project}", // DCP
    "env"       , "${var.env}" // Stage {dev,integration,staging,prod}
    "service"   , "${var.service}", 
    "owner"     , "${var.owner}" email, should be populated from aws sts.idenity
  )}"   
}
```

```
// only the defualt tags
resource aws_s3_bucket dss_s3_bucket_test_fixtures {
  count = "${var.DSS_DEPLOYMENT_STAGE == "dev" ? 1 : 0}"
  bucket = "${var.DSS_S3_BUCKET_TEST_FIXTURES}"
  tags = "${local.common_tags}"
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

Once a tag set for a resource has been established it should be updated within the [DCP Resource Tagging wiki](https://allspark.dev.data.humancellatlas.org/dcp-ops/docs/wikis/Resource%20Tagging) so possible configureations can be seen.

 // TODO : amar: see if the cost explorer allows all values for the Tag Key to be added to the fitler vs having the add them manually by the admin.


### Acceptance Criteria [optional]

A set of tags/labels is defined and required for Cloud resources to have.

### Unresolved Questions

Are there other tags that we need to have?
Are there other resources that should have tags become required for them.


### Drawbacks and Limitations [optional]

*Why should this RFC **not** be implemented?*
if we want the admins to not like the devs.


### Alternatives [optional]

Services have independent tags that are posted within the Resource Tagging wiki, and admins have to track them down.
