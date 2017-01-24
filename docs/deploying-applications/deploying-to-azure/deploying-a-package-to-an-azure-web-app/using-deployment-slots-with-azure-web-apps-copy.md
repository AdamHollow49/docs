---
title: Using Deployment Slots with Azure Web Apps copy

---


[Deployment Slots](https://azure.microsoft.com/en-us/documentation/articles/web-sites-staged-publishing/) provide a nice way to implement [Blue-Green deployments](http://martinfowler.com/bliki/BlueGreenDeployment.html) for Azure Web Apps.


This provides many benefits, including:

- Reduced down-time when deploying.
- When deploying packages with a large number of files, deployment times can be significantly reduced due to not having to compare with existing files (this assumes you are deploying to a clean slot).
- Roll-back can be achieved by simply swapping the slots back.





:::warning
Deployment Slots are only available to Azure Web Apps running in Standard or Premium[App Service plans](https://azure.microsoft.com/en-us/pricing/details/app-service/plans/)
:::





See the [Azure Web Blue-Green Sample project](https://demo.octopusdeploy.com/app#/projects/azure-web-blue-green-sample/process) on our demo server, for a complete example.

## Walk-Through


Here we will give an example of how to setup a Blue-Green deployment for an Azure Web App using Deployment Slots.

### Step 1: Create Staging Slot


Create a [Run an Azure PowerShell Script](/docs/guides/azure-deployments/running-azure-powershell/index.md) step. For information about adding a step to the deployment process, see the [add step](http://docs.octopusdeploy.com/display/OD/Add+step) section.


![](/docs/images/5671696/5865912.png "width=170")


Assuming you have a variable named 'WebSite' that contains the name of your Azure Web Site, your script should be:

**Azure Service Management**

```powershell
#Remove the staging slot if it exists
Remove-AzureWebsite -Name #{WebSite} -Slot Staging -Force

#Create the staging slot
New-AzureWebsite -Name #{WebSite} -Slot Staging
```

**Azure Resource Manager**

```powershell
#Remove the staging slot if it exists
Remove-AzureRMWebAppSlot -Name #{WebSite} -Slot Staging -ResourceGroupName MyResourceGroup -Force

#Create the staging slot
New-AzureRMWebAppSlot -Name #{WebSite} -Slot Staging -ResourceGroupName MyResourceGroup
```

:::hint
The reason for the first line, which removes the Staging Slot, is to ensure we are deploying to a clean slot.  This can significantly reduce the time taken for deployments with a large number of files.
:::





So your step should look like:


![](/docs/images/5671833/5866079.png "width=500")




### Step 2: Deploy your Package


The next step is to deploy your package to the Staging slot.  We do this be creating a [Deploy an Azure Web App](/docs/deploying-applications/deploying-to-azure/deploying-a-package-to-an-azure-web-app/index.md) step.


![](/docs/images/5671833/5866078.png "width=500")




:::hint
Slots in Azure are themselves real Web Apps with their own hostnames.  They are named with the format:

```
WebsiteName(SlotName) 
```
:::





If your Staging Slot already exists, you can see it in the select-list of available sites for your account.


![](/docs/images/5671833/5866077.png "width=500")





To use a variable for the WebSite name, check the "Use a Custom Expression" box, and enter

```
#{WebsiteName}(Staging)
```


As shown below:


![](/docs/images/5671833/5866076.png "width=500")










### Step 3: Swap the Staging and Production Slots


The final step is to create another Azure PowerShell step to swap the Staging and Production slots.


Use the PowerShell:

**Azure Service Management**

```powershell
#Swap the staging slot into production
Switch-AzureWebsiteSlot -Name #{WebSite} -Slot1 Staging -Slot2 Production -Force
```

**Azure Resource Management**

```powershell
#Swap the staging slot into production
$ParametersObject = @{targetSlot  = "Production"}
Invoke-AzureRmResourceAction -ResourceGroupName MyResourceGroup -ResourceType Microsoft.Web/sites/slots -ResourceName #{WebSite}/Staging -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```


So your step will appear as:


![](/docs/images/5671833/5866075.png "width=500")





At this point you should have a working Blue-Green deployment process for your Azure Web App.


![](/docs/images/5671833/5866074.png "width=500")
