---
title: Web App accounts

---


Deploying a Web App in Octopus requires the configuration of an Azure Subscription Account.  Azure Subscription Accounts contain the details of Azure subscriptions.

## Creating the account {#WebAppaccounts-Creatingtheaccount}


![](/docs/images/3049434/3278565.png "width=500")

### Subscription Id {#WebAppaccounts-SubscriptionId}


The subscription Id can be found on the Settings tab of the Azure Management portal.


![](/docs/images/3049434/3278564.png "width=500")




### Management certificate {#WebAppaccounts-Managementcertificate}


Octopus Deploy must authenticate to Azure using an X.509 certificate.  You can either upload an existing certificate (`.pfx`), or leave the field blank and Octopus Deploy will generate a certificate. Keep in mind that since Octopus Deploy securely stores the certificate internally, there is no need to upload a password protected `.pfx`file. If you would like to use one that is password protected, you will need to first remove the password. This can be done with the following commands.

**Remove .pfx password**

```bash
openssl pkcs12 -in AzureCert.pfx -password pass:MySecret -nodes -out temp.pem
openssl pkcs12 -export -in temp.pem -passout pass -out PasswordFreeAzureCert.pfx
del temp.pem
```


If you allow Octopus Deploy to generate your certificate, you will need to upload the certificate to the Azure Management Portal.  After clicking 'Save', the Account settings page provides instructions for downloading the certificate public-key from Octopus Deploy, and uploading it into the Azure Management Portal.


![](/docs/images/3049434/3278566.png "width=500")


Uploaded certificates can be viewed on the 'Management Certificates' tab of the 'Settings' page in the Azure Management Portal.


![](/docs/images/3049434/3278567.png "width=500")


The certificate will be named `Octopus Deploy -``{Your Account Name}.`
