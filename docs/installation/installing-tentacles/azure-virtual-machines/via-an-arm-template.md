---
title: Installing the Tentacle VM Extension via an ARM Template
description: How to install the Tentacle VM Extension using an Azure Resource Manager (ARM) Template
position: 5
---

:::hint
The Azure VM Extension is currently in preview.
:::

You can deploy the extension at virtual machine creation time, or update an extension resource group to apply the extension later.

Create your ARM template as normal, and add a `resources` element under your `Microsoft.Compute/virtualMachine` resource:

```json
"resources": [
  {
    "type": "extensions",
    "name": "[concat(parameters('vmName'),'/OctopusDeployWindowsTentacle')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[resourceGroup().location]",
    "dependsOn": [
      "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
    ],
    "properties": {
      "publisher": "OctopusDeploy.Tentacle",
      "type": "OctopusDeployWindowsTentacle",
      "typeHandlerVersion": "2.0",
      "autoUpgradeMinorVersion": "true",
      "forceUpdateTag": "1.0",
      "settings": {
        "OctopusServerUrl": "http://localhost:81",
        "Environments": [
          "Development",
          "Staging"
        ],
        "Roles": [
          "App Server",
          "Web Server"
        ],
        "CommunicationMode": "Listen",
        "Port": 10933,
        "PublicHostNameConfiguration": "PublicIP"
      },
      "protectedSettings": {
        "ApiKey": "API-ABCDEF1234567890ABCDEF12345"
      }
    }
  }
]
```

## Properties ##

* `publisher`: (string) Must be `OctopusDeploy.Tentacle`.
* `type`: (string) Must be `OctopusDeployWindowsTentacle`.
* `typeHandlerVersion`: (string): The major and minor version of the extension to apply. You can find what versions are available [via the Azure CLI](via-the-azure-cli).
* `autoUpgradeMinorVersion`: (string) Indicates whether the extension version should be automatically updated to a newer minor version. Accepts the values `true` or `false`.
* `forceUpdateTag`: (string) Any user defined value that can be modified to force the extension to re-run, even if none of the `settings` or `protectedSettings` have changed.


Please refer to the [configuration structure](configuration-structure.md) for details regarding the format of the `settings` and `protectedSettings` elements.

