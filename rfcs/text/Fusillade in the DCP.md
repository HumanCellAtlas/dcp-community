### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# Fusillade Management Processes

*Replace "RFC Name" above with the name of your RFC. Keep it short and descriptive.*

## Summary

Define a series of process for managing fusillade across the components of the DCP.

## Author(s)

*Recommended format for Authors:*

 `[Trent Smith](mailto:trent.smith@chanzuckerberg.com)`

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

Fusillade is responsible for providing a uniform authentication and authorization experience across the DCP. As the DCP 
moves forward with integrating fusillade into the DCP, processes needs to be put into place to prevent excessive 
breaking of the system due to permission errors. By agreeing on a process for managing the permission in Fusillade, 
errors relating to permissions can be avoided, and will be easier to track down when they occur. 

The goal of having an agreed process is to simplify the manage of Fusillade Roles, Groups, Users, Service Accounts 
across 
the DCP.
 
[fusillade:#226](https://github.com/HumanCellAtlas/fusillade/issues/226)

### User Stories

1. As a component of the DCP, I would like to define permissions to limit access to specific resources.
1. As a component of the DCP, I would like to define a group of users to manage the permission of a large groups of 
users easily.
1. As a component of the DCP, I would like to give permission to unauthenticated users.
1. As a component of the DCP, I would like to use service account to programmatically manage my service.  
1. [fusillade:#226](https://github.com/HumanCellAtlas/fusillade/issues/226)
## Scientific "guardrails" [optional]

*Describe recommended or mandated review from HCA Science governance to ensure that the RFC addresses the needs of the scientific community.*

## Detailed Design
How do we want to use groups and roles in the DCP? 

We need to define some basic groups for assigning our users, this list of groups should be in a central location and the roles associated with them are defined by each component. Components are responsible for managing those roles and ensuring they are available when the group is created and modified.

### Component Organization Convention
Components will have a directory at the top level of their code base named */roles*. This directory will contain all 
of the roles defined for this component. This the directory structure:

```
project
+-- roles
|   +-- admin.json
|   +-- wrangler.json
```
Having the roles defined in a common location across components will make it easier for developers to find what roles
 exists for each component.
 
### DCP Fusillade Organization Management Convention
A directory name */fusillade* will contain a */groups* directory with all of the groups.
Initially the *groups* directory will only have one group which is `user_default`. All users when they are first 
created are added to this group. This is the directory structure:

```
project
+-- fusillade
|   +-- groups
|   |   +-- user_default.json
|   |   +-- group_1.json
|   |   +-- group_2.json
```

### Guidelines


#### Group and Role Name Convention
For production deployments stage should be excluded.
- For component specific groups, name it as follows: `{component}-{name}`
- For DCP wide groups, name it as follows: `DCP-{name}`

For all stages other than production follow this naming scheme.
- For component specific groups, name it as follows: `{component}-{stage}-{name}`
- For DCP wide groups, name it as follows: `DCP-{stage}-{name}`

**Note:** `group/user_default`, `role/fusillade_admin`, and `role/default_user` do not follow this naming convention.
 See [using fusillade as a service](https://github.com/HumanCellAtlas/fusillade#using-fusillade-as-a-service) for 
 more information.

### Component Dev Environment
DCP Components should use the Fusillade staging environment to test their dev code. This is to ensure relative 
stability when developing for the DCP. Follow the naming convention for groups and roles to prevent name collision 
between dev and staging deployments.

Alternatively a new fusillade deployment cloud be made just for component Dev Testing.

**Note:** The fusillade managed group `user_default` and user `public` will be modified for both your components 
staging and dev environment.

### Resource
For resource set the *partition* to `hca`, set your *service* name to the name of your component, and set the 
*account-id* to the deployment stage. All other fields can be used as needed or use \* for wild cards. resource names
 are case sensitive. See [fusillade policies](https://github.com/HumanCellAtlas/fusillade#policies) to learn more.

#### Examples

- **arn**:**hca**:**fusillade**:region:**dev**:resource
- **arn**:**hca**:**dss**:region:**staging**:resourcetype/resource
- **arn**:**hca**:**query**:region:**integration**:resourcetype/resource/qualifier
- **arn**:**hca**:**ingest**:region:**prod**:resourcetype/resource:qualifier
- **arn**:**hca**:**azul**:region:**dev**:resourcetype:resource
- **arn**:**hca**:**matrix**:region:**staging**:resourcetype:resource:qualifier

All fields in **bold** must be used. All other fields are free to use by the component.

### Tools Needed

#### Role Manager (I did not spend a lot of time on naming.)
```
$ DCPRole.py --help

    Use to add or modify roles in fusillade for the DCP.
     
    --file      "path to a {role}.json file"
    --component "name of the component" 
    --stage     "current deployment stage" 
    --secret    "secretId of a service account in AWS" 
    --service   "path to service account credentials" 
    --force     "update the role if the role exists"
```

#### Group Manager (I did not spend a lot of time on naming.)
```
$ DCPGroup.py --help

    Use to add or modify groups in fusillade for the DCP. Users specified in members will be created and/or added to 
    the specified group. Roles specified will be added to the group if they exist.
    
    --file      "path to a {role}.json file"
    --component "name of the component" 
    --stage     "current deployment stage" 
    --secret    "secretId of a service account in AWS" 
    --service   "path to service account credentials" 
```

### How To?

#### As a component of the DCP, how do I control access to my resources?
1. Define what actions and resources you'd like to control. Actions should take the form `{component}:{Action}`, for example to get a bundle from the DSS the action could be `dss:GetBundle`. The action is case sensitive, and camel case is suggested. Resources are what actions affect. The same action can be applied to multiple different resources. For the resource use the AWS ARN format. For example...

1. Define the different types of users you need to support based on what actions and resource they will need permission for. These users types you define will be your roles. 

1. Create a file **{role}.json** for each user type, where `{role}` is the name of your role. Using the resource and actions previously define, write a policy in the AWS IAM policy format that express what each role can do. If it isn't explicitly stated, it is assumed the user with this role does not have permission. For the resource field, `*` can be used as a wild card in the same way it's used in AWS IAM policies.

1. Run the script `DCPRole.py` to add your roles to fusillade. The role that is added to fusillade will be named `{component}-{stage}-{role}`. 
The stage is not included for *prod* deployments. If the role already existed an error will be thrown. Use the 
parameter `--force` to overwrite the existing role. 

1. Users can now be assigned to this role. However the preferred method of adding users to a role is to assign this 
role to a group. In the DCP, groups are managed in dcpinfra repo. 

#### As a DCP operator how do I had groups to the DCP?
A service account with permission to create and modify fusillade groups is required to perform this process.

1. In *humancellatlas/dcpinfra/fusillade/groups* create a file name `{group}.json` where `{group}` is the name of your group. Here is an example of the contents of a *{group}.json* file 
```JSON
{
      "name": "name_of_the_group",
      "description": "a description of the group",
      "members": [
        "a_list_of_user_emails@example.example",
        "a_list_of_service_account_emails@example.example"
      ],
      "roles": [
        "the_roles_assigned_to_this_group"
      ]
}
``` 

1. In DCP infra run the script `DCPGroup.py` to add your group to Fusillade. If any roles 
specified in the group do not exist, an error will be thrown, and that group will not be created if it did not 
previously exist, or it will not be updated if previously existed. Any member not in the group will be added to the 
group. The group added will be named `{group}-{stage}`. `{stage}` is excluded from name when deployment is 
`prod`. The group named `user_default` will not contain the deployment stage when it is add/modified.

#### As a DCP operator how do I remove a role from a group?
1. With an authenticated user or service account with permissions to modify groups, use the PUT 
/v1/group/{group_id}/roles endpoint to remove a role from a group.

#### As a DCP operator how do I remove a user from a group?
1. With an authenticated user or service account with permissions to modify users, use the PUT 
/v1/user/{user_id}/groups endpoint to remove a user from a group.

#### As a DCP operator how do I give a user or service account permissions?
1. Add the user or service account as a member of the group with the desired permissions.

#### As a DCP component how do I give all new users a default level of access to the DCP?
1. In *humancellatlas/dcpinfra/fusillade/groups* add a role to `default_user.json` that provides the 
permissions for all new users.

#### As a DCP component how do I test my permissions?
1. Create a service account with permission to use the fusillade `POST /v1/policies/evaluate` endpoint.
1. Create a role and add it to the fusillade deployment
1. Assign the roles to a user directly, or to a group with the user is a member.
1. Using an authorized service account make a request to fusillade `POST /v1/policies/evaluate` with a user assigned 
that role as the principal in the request.

### Acceptance Criteria [optional]

*Acceptance criteria are the conditions that a RFC must satisfy to be accepted by users or other stakeholders.* 

### Unresolved Questions

- Does this process work for all of the existing components deployment process?
- Does this provide enough guidance for components to being integrating fusillade?
- What environment should be used for Component development testing? Should a new one be created? 
- *What aspects of the design do you expect to clarify later during iterative development of this RFC?*

### Drawbacks and Limitations [optional]

- This will add additional process and impose additional conventions on the DCP.
- Component roles and DCP groups live in separate repositories. This will require two PR add a new role, but only one
 PR to modify an existing role.
 
- Having components use both fusillade staging to test thier dev and staging environments could be problematic.

### Prior Art [optional]

*Share references to prior art to deepen community understanding of the RFC, such as learnings, adaptations from earlier designs, or community standards.*

### Alternatives [optional]
*Highlight other possible approaches to delivering the value proposed in this RFC. 

- Manage fusillade configuration in each component repo. Components will need to be careful to interfere with other 
components permissions.