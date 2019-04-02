### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# RFC Name

Resource Tagging

## Summary

Define a standard for Cloud Resource tagging to allow normilzation of data for Cost analysis.

*Describe the vision of the RFC in 2-3 sentences. Consider this section as your "elevator pitch" or "release note" to the community.*

## Author(s)

*Recommended format for Authors:*

 `[Amar Jandu](mailto:ajandu@ucsc.edu)`

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

*Describe the user or technical need in detail [with alignment to the DCP roadmap priorities where possible]. Link prior community discussions to demonstrate support for this RFC.*

AWS Cost Explorer requires managment for allowing tags to track and show to PMs/Admins. Fragmented tag sets create unneccessary overhead for the admin team, as well as present issues with filtering the information downstream. 

### User Stories

*Share the [User Stories](https://www.mountaingoatsoftware.com/agile/user-stories) motivating this RFC.*
As a DCP Admin, I would like to see costs for a given project/service, so that i can track costs.

## Scientific "guardrails" [optional]

*Describe recommended or mandated review from HCA Science governance to ensure that the RFC addresses the needs of the scientific community.*

## Detailed Design

*Explain the design in sufficient detail such that the implementation and (if appropriate) the interaction with existing DCP software are both reasonably clear.*

For Cloud Resources that are [allowed](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/custom-tags.html#allocation-how) to be tagged, it should be encuraged that they are, however a subset of the resources should have tagging as a requirement.

Required Resource Tags for AWS these include `lambda, apigateway, s3, ec2, rds, elasticsearch service, elb, ecs`. There are othere related technologie that may also beifit from tagging such as `vpc,ebs` for `ec2`, resources such as these may also benifit from tagging to further clarify ownership.

Required labels for GCP include ``

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

*Acceptance criteria are the conditions that a RFC must satisfy to be accepted by users or other stakeholders.*

A set of tags/labels is defined and required for Cloud resources to have.

### Unresolved Questions

- *What aspects of the design do you expect to clarify further through the RFC review process?*
- *What aspects of the design do you expect to clarify later during iterative development of this RFC?*
Are there other tags that we need to have?
Are there other resources that should have tags become required for them.


### Drawbacks and Limitations [optional]

*Why should this RFC **not** be implemented?*
if we want the admins to not like the devs.


### Prior Art [optional]

*Share references to prior art to deepen community understanding of the RFC, such as learnings, adaptations from earlier designs, or community standards.*



### Alternatives [optional]

*Highlight other possible approaches to delivering the value proposed in this RFC. 
What other designs were explored? What were their advantages? What was the rationale for rejecting alternatives?*

Services have independent tags that are posted within the Resource Tagging wiki, and admins have to track them down.
