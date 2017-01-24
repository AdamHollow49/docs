---
title: User Roles

---


User Roles play a major part in the Octopus security model. These roles are assigned to Teams and they dictate what the members of those teams can do in Octopus.


- Built-in User Roles
- Creating User Roles
- Troubleshooting Permissions

## Built-in User Roles


Octopus comes with a set of built-in User Roles that are designed to work for most common scenarios:

| User Role | Description |
| --- | --- |
| Environment Manager | Environment managers can view and edit environments and their machines. |
| Environment Viewer | Environment viewers can view environments and their machines, but not edit them. |
| Package Publisher | Permits packages to be pushed to the Octopus server's built-in NuGet feed. |
| Project Viewer | Project viewers have read-only access to a project. They can see a project in their dashboard, view releases and deployments. Restrict this role by project to limit it to a subset of projects, and restrict it by environment to limit which environments they can view deployments to. |
| Project Contributor | All project viewer permissions, plus: editing and viewing variables, editing the deployment steps. Project contributors can't create or deploy releases. |
| Project Initiator | All project viewer permissions, plus: create new projects. |
| Project Deployer | All project contributor permissions, plus: deploying releases, but not creating them. |
| Project Lead | 

All project contributor permissions, plus: creating releases, but not deploying them.
 |
| System Administrator | System administrators can do everything. |





The built-in User Roles can be modified to contain more or less roles to suit specific needs. But instead of modifying the built-in ones, we recommend that you leave them as an example and instead create your own User Roles.

## Creating User Roles


A custom User Role can be created with any combination of roles. To create a custom user role:

1. Go to the Teams page, and look for the Roles link**![](/docs/images/3048105/3278287.png "width=500")**
2. Click **Add custom role**
![](/docs/images/3048105/3278288.png "width=500")
3. Select the set of roles you'd like this new User Role to contain, and give the role a name and description
![](/docs/images/3048105/3278289.png "width=500")



Once the custom role is saved, the new role will be available to be assigned to any team on Octopus.


![](/docs/images/3048105/3278290.png "width=500")

## Troubleshooting Permissions


If for some reason a user has more/fewer permissions than they should, you can use the **Test Permissions** feature to get an easy to read list of all the permissions that a specific user has on the Octopus instance.


To test the permissions go to **Configuration** -> **Teams** -> **Test Permissions** and select a user from the dropdown.


The results will show

- The teams of which the user is a member of.
- A chart detailing each role and on which Environment/Project this permission can be executed. The chart can be exported to a CSV file by clicking the Export button. Once the file is downloaded it can viewed in browser using [Online CSV Editor and Viewer](http://www.convertcsv.com/csv-viewer-editor.htm).






![](https://cloud.githubusercontent.com/assets/347637/15734434/fae1f98c-28d2-11e6-8440-bdb6eec98609.png "width=500")


If a user tries to perform an action without having enough permissions to do it, an error message will pop up showing which permissions the user is lacking, and which teams actually have these permissions.


![](/docs/images/3047940/3277151.jpg "width=500")

:::warning
As further versions of Octopus are released, we might create new roles to improve our security model. These new roles will not be automatically included in any of the built-in User Roles, to avoid giving users permissions they are not supposed to have. These new roles will have to be added manually to a User Role by an administrator.
:::
