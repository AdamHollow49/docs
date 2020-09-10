---
title: List users with role
description: An example script to list all users that have a specific role by team.
---

This script will list all users with a given role by team.  You can also filter the list by specifying a space name.

## Usage

Provide values for:

- Octopus URL
- Octopus API Key
- User Role Name
- (Optional) Space Name

## Example output

### All spaces with role name `Project Deployer`

Team: Build Servers  
Space: Default  
TeamCity  
Build Server  
Team: Can Deploy But Not Download Packages  
Space: Default  
PackageTest  
Team: Developer Lower Environment  
Space: Default  
Paul Oliver the Developer  
Team: Devs  
Space: Default  
Team: Quick Test  
Space: Default  
Ryan Rousseau  
Team: ShawnTest  
Space: AzureDevOps  
Adam Close  
External security groups:  
TestDomain\SpecialGroup

### Space name AzureDevOps

Team: ShawnTest  
Space: AzureDevOps  
Adam Close  
External security groups:  
TestDomain\SpecialGroup  

## Scripts

!include <list-users-with-role-scripts>
