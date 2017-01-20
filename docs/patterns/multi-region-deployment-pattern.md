---
title: Multi-region deployment pattern
position: 6
---


## Scenario


Your application is deployed to multiple geographic regions (or multiple data centres) to provide for your end-customer's performance (think latency) or legal requirements (like data sovereignty).


![](/docs/images/5670886/5865791.png)

## Strict solution using Environments


You can use�[Environments](/docs/home/key-concepts/environments.md)�to represent each region or data centre. In the example below we have defined a Dev and Test Environment as per normal, and then configured two "production" Environments, one for each region we want to deploy into.


![](/docs/images/5670886/5865781.png)


By using this pattern you can:

1. Use�[Lifecycles](/docs/home/key-concepts/lifecycles.md)�to define a strict process for promotion of releases between your regions.�*Lifecycles can be used to design both simple and complex promotion processes.*
 1. For example, you may want to test releases in Australia before rolling them out to the USA, and then to Europe
 2. In another example, you may want to test releases in Australia before rolling them out simultaneously to all other regions
2. Scope region-specific variables to the region-specific Environments
3. Quickly see which releases are deployed to which regions on the main dashboard
4. Quickly promote releases through your regions using the Project Overview
5. Use [Scheduled Deployments](/docs/home/deploying-applications/scheduled-deployments.md) to plan deployments for times of low usage



**This is a really good solution if you want to enforce a particular order of deployments through your regions.**




## Rolling Solution


In Octopus 3.4 we introduced�[Cloud Regions](/docs/home/deployment-targets/cloud-regions.md) which enable you to configure�[Rolling deployments](/docs/home/patterns/rolling-deployments.md) across your regions or data centres. In this case you can scope variables to the Cloud Regions and deploy to all regions at once, but you cannot control the order in which the rolling deployment executes.


![](/docs/images/5670886/5865782.png)


By using this pattern you can:

1. Scope region-specific variables to the Cloud Region targets
2. Conveniently deploy to all regions at the same time



**If you don't really mind which order you regions are deployed, or you always upgrade all regions a the same time, Cloud Regions are probably the right fit for you.**

## Tenanted Solution


Alternatively you could create�[Tenants](/docs/home/key-concepts/tenants.md) to represent each region or data centre. By doing so you can:

1. Use�[Variable Templates](/docs/home/deploying-applications/variables/variable-templates.md) to prompt you for the variables required for each region (like the storage account details for that region) and when you introduce a new region Octopus will prompt you for the missing variables
![](/docs/images/5670886/5865790.png)
2. Provide logos for your regions to make them easier to distinguish
![](/docs/images/5670886/5865788.png)
3. Quickly see the progress of deploying the latest release to your entire production environment on the main dashboard
![](/docs/images/5670886/5865785.png)
4. Quickly see�which releases have been deployed to which regions using the Dashboard and Project Overview
![](/docs/images/5670886/5865786.png)
5. Quickly promote releases to your production regions, in a particular sequence, or simultaneously
![](/docs/images/5670886/5865789.png)
6. Use�[Scheduled Deployments](/docs/home/deploying-applications/scheduled-deployments.md) to plan deployments for times of low usage
![](/docs/images/5670886/5865787.png)






You do give up the advantage of enforcing the order in which you deploy your application to your regions, but you gain the flexibility to promote to your regions in different order depending on the circumstances.


**Tenants offer a balanced approach to modelling multi-region deployments, offering a measure of control and flexibility.**

## Conclusion


[Environments](/docs/home/key-concepts/environments.md),�[Tenants](/docs/home/key-concepts/tenants.md) and�[Cloud Regions](/docs/home/deployment-targets/cloud-regions.md)�can be used to model multi-region deployments in Octopus, but each different choice is optimized to a particular style of situation. Choose the one that suits your needs best!
