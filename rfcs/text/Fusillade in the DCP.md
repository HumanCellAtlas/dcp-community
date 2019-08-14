### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# Fusillade Management Processes

*Replace "RFC Name" above with the name of your RFC. Keep it short and descriptive.*

## Summary

Define a series of process for managing fusillade across the components of the DCP.

## Author(s)

*Recommended format for Authors:*

 `[Trent Smitg](mailto:trent.smith@chanzuckerberg.com)`

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

Fusillade is responsible for providing a uniform authentication and authorization experiance across the DCP. As the DCP 
moves forward with integrating fusillade into the DCP, processes needs to be put into place to prevent excessive 
breaking of the system due to permission errors. By agreeing on a process for managing the permission in Fusillade, 
errors relating to permissioning can be avoided, and will be easier to track down when they occur. 

The goal of have an agreed process is to simplify the manage of Fusillade Roles, Groups, Users, Service Accounts across 
the DCP.
 
[fusillade:#226](https://github.com/HumanCellAtlas/fusillade/issues/226)

### Glossary

### User Stories

1. As a component of the DCP, I would like to define permissions to limit access to specific resources.
1. As a component of the DCP, I would like to define a group of users to manage the permission of a large groups of 
users easily.
1. As a component of the DCP, I would like to give permission to unauthenticated users.
1. As a component of the DCP, I would like to use service account to programmatically manage my service.  

## Scientific "guardrails" [optional]

*Describe recommended or mandated review from HCA Science governance to ensure that the RFC addresses the needs of the scientific community.*

## Detailed Design

### As a component of the DCP, how do I control access to my resources?
1. Define what actions and resources you'd like to control. Actions should take the form `{component}:{Action}`, for example to get a bundle from the DSS the action could be `dss:GetBundle`. The action is case sensitive, and camel case is suggested. Resources are what actions affect. The same action can be applied to multiple different resources. For the resource use the AWS ARN format. For example...

1. Define the different types of users you need to support based on what actions and resource they will need permission for. These users types you define will be your roles. 

1. Create a file **{role}.json** for each user type. Using the resource and actions previously define, write a policy in the AWS IAM policy format that express what each role can do. If it isn't explicitly stated, it is assumed the user with this role does not have permission. For the resource field, `*` can be used as a wild card in the same way it's used in AWS IAM policies.

1. Run the script `??? -path "path to roles" -component "name of the component" -stage "current deployment stage" -secret "secretId of a service account in AWS" -service "path to service account credentials"` to add your roles to fusillade. The role that is added to fusillade will be named `{component}-{stage}-{role}`. The stage is not included for *prod* deployments.

1. Users can now be assigned to this role. However the preferred method of adding users to a role is to assign this role to a group. In the DCP, groups are manage in dcpinfra repo. 

### As a DCP operator how do I had groups to the DCP?
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

1. In DCP infra run the script `??? -path "path to groups" -stage "current deployment stage" -secret "secretId of a 
service account in AWS" -service "path to service account credentials"` to add your group to Fusillade. If any roles 
specified in the group do not exists, an error will ne thrown and that group will not be create if it didn't exist, 
or it will not be updated if it existed. Any member not in the group will be added to the group. The group added will
 be named `{group}-{deployment}`. `{deployment}` is excluded from name when deployment is `prod`. For all deployments
  the group named `user_default` does is not contain the deployment.

### As a DCP operator how do I remove a role from a group?
1. With an authenticated user or service account with permissions to modify groups, use the PUT 
/v1/group/{group_id}/roles endpoint to remove a role from a group.

### As a DCP operator how do I remove a user from a group?
1. With an authenticated user or service account with permissions to modify users, use the PUT 
/v1/user/{user_id}/groups endpoint to remove a user from a group.

### As a DCP operator how do I give a user or service account permissions?
1. Add the user or service account as a member of the group with the desired permissions.

### As a DCP component how do I give all new users a default level of access to the DCP?
1. In *humancellatlas/dcpinfra/fusillade/groups* add a role to `default_user.json` that provides the 
permissions for all new users.
