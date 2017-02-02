---
title: System variables
position: 5
---

This page lists built-in [variables](/docs/deploying-applications/variables/index.md) provided by Octopus that can be used in your deployment [Custom scripts](/docs/deploying-applications/custom-scripts/index.md).

## Deployment {#Systemvariables-Deployment}

Deployment-level variables are drawn from the project and release being deployed.

<div class="table-responsive">
  <table class="table">
    <tr><th>Name</th><th>Example</th><th>Description</th></tr>
    <tr><td>Octopus.Acquire.MaxParallelism</td><td>*2*</td><td>Controls the number of package acquisitions that will be allowed to run concurrently.</td></tr>
    <tr><td>Octopus.Acquire.MaxParallelism           </td><td> *2*                                      </td><td> Controls the number of package acquisitions that will be allowed to run concurrently. </td></tr>
    <tr><td>Octopus.Acquire.DeltaCompressionEnabled  </td><td> true                                     </td><td> Toggle whether delta compression is enabled when sending packages to targets. </td></tr>
    <tr><td>Octopus.Deployment.Comments              </td><td> *Signed off by Alice*                    </td><td> User-provided comments on the deployment </td></tr>
    <tr><td>Octopus.Deployment.Created               </td><td> *Tuesday 10th September 1:23 PM*         </td><td> The date and time at which the deployment was created </td></tr>
    <tr><td>Octopus.Deployment.CreatedBy.DisplayName </td><td> *Alice King*                             </td><td> The full name of the user who initiated the deployment </td></tr>
    <tr><td>Octopus.Deployment.CreatedBy.EmailAddress</td><td> *[alice@example.com](mailto:alice@example.com)* </td></tr> The email address of the user who initiated the deployment </td></tr>
    <tr><td>Octopus.Deployment.CreatedBy.Id          </td><td> *users-123*                              </td><td> The ID of the user who initiated the deployment </td></tr>
    <tr><td>Octopus.Deployment.CreatedBy.Username    </td><td> *alice*                                  </td><td> The username of the user who initiated the deployment </td></tr>
    <tr><td>Octopus.Deployment.Error                 </td><td> *Script returned exit code 123*          </td><td> This variable outputs the error/exit code for a failed deployment. [See here](/docs/deploying-applications/variables/system-variables.md) </td></tr>
    <tr><td>Octopus.Deployment.ErrorDetail           </td><td> *System.IO.FileNotFoundException: file C:\Missing.txt does not exist (at...)* </td></tr> The error/exit code for the deployment along with the Octopus stack trace. [See here](/docs/deploying-applications/variables/system-variables.md) </td></tr>
    <tr><td>Octopus.Deployment.ForcePackageDownload  </td><td> *False*                                  </td><td> If true, the package will be freshly downloaded from the feed/repository regardless of whether it is already present on the endpoint *(Boolean)* </td></tr>
    <tr><td>Octopus.Deployment.Id                    </td><td> *deployments-123*                        </td><td> The ID of the deployment                 </td></tr>
    <tr><td>Octopus.Deployment.Name                  </td><td> *Deploy to Production*                   </td><td> The name of the deployment               </td></tr>
    <tr><td>Octopus.Deployment.PreviousSuccessful.Id </td><td> *deployments-122*                        </td><td> The ID of the previous successful deployment of this project in the target environment </td></tr>
    <tr><td>Octopus.Deployment.Machines              </td><td> machines-123,machines-124                </td><td> Ids of machines being targeted by the deployment. </td></tr>
    <tr><td>Octopus.Deployment.SpecificMachines      </td><td> *machines-123,machines-124*              </td><td> Specific machines being targeted by the deployment, if any*(List)* </td></tr>
    <tr><td>Octopus.Deployment.ExcludedMachines      </td><td> machines-123,machines-124                </td><td> Ids of machines that have been excluded from the deployment (generally for being unavailable) </td></tr>
    <tr><td>Octopus.Deployment.Tenant.Id             </td><td> *Tenants-123*                            </td><td> The ID of the Tenant being deployed for. If the deployment is untenanted (or pre 3.4.0) then this variable will not be present. </td></tr>
    <tr><td>Octopus.Deployment.Tenant.Name           </td><td> *Acme Corp*                              </td><td> The name of the Tenant being deployed for. If the deployment is untenanted (or pre 3.4.0) then this variable will not be present. </td></tr>
    <tr><td>Octopus.Deployment.Tenant.Tags           </td><td> *Tenant type/External, Upgrade ring/Early adopter* </td><td> Comma delimited list of tags that belong the the Tenant being deployed for. If the deployment is untenanted (or pre 3.4.0) then this variable will not be present. </td></tr>
    <tr><td>Octopus.Endpoint.\_type\_.\_property\_   </td><td> *ftp.example.com*                        </td><td> Properties describing the endpoint being deployed </td></tr>
    <tr><td>Octopus.Environment.Id                   </td><td> *environments-123*                       </td><td> The ID of the environment                </td></tr>
    <tr><td>Octopus.Environment.MachinesInRole[\_role\_] </td><td> *machines-123,machines-124*              </td><td> Lists the machines in a specified role   </td></tr>
    <tr><td>Octopus.Environment.Name                 </td><td> *Production*                             </td><td> The name of the environment              </td></tr>
    <tr><td>Octopus.Environment.SortOrder            </td><td> *3*                                      </td><td> The ordering applied to the environment when it is displayed on the dashboard and elsewhere </td></tr>
    <tr><td>Octopus.Machine.Id                       </td><td> *machines-123*                           </td><td> The ID of the machine                    </td></tr>
    <tr><td>Octopus.Machine.Name                     </td><td> *WEBSVR01*                               </td><td> The name that was used to register the machine in Octopus. Not the same as *Hostname* </td></tr>
    <tr><td>Octopus.Machine.Roles                    </td><td> *web-server,frontend*                    </td><td> The roles applied to the machine *(List)* </td></tr>
    <tr><td>Octopus.Machine.Hostname                 </td><td> Database01, www.Database01.local, 192.168.200.100 </td><td> The host part of the URI that was used to register the machine, could be an IP, hostname depending on what was supplied. Only set for Listening Tentacles </td></tr>
    <tr><td>Octopus.Project.Id                       </td><td> *projects-123*                           </td><td> The ID of the project                    </td></tr>
    <tr><td>Octopus.Project.Name                     </td><td> *OctoFx*                                 </td><td> The name of the project                  </td></tr>
    <tr><td>Octopus.ProjectGroup.Id                  </td><td> *projectgroups-123*                      </td><td> The ID of the projectgroup               </td></tr>
    <tr><td>Octopus.ProjectGroup.Name                </td><td> *Public Web Properties*                  </td><td> The name of the projectgroup             </td></tr>
    <tr><td>Octopus.Release.Channel.Name             </td><td> *2.x Feature Branch*                     </td><td> The Channel name associated with the release </td></tr>
    <tr><td>Octopus.Release.Notes                    </td><td> *Fixes bugs 1, 2 & 3*                    </td><td> Release notes associated with the release, in Markdown format </td></tr>
    <tr><td>Octopus.Release.Number                   </td><td> *1.2.3*                                  </td><td> The version number of the release        </td></tr>
    <tr><td>Octopus.Release.Previous.Id              </td><td> *releases-122*                           </td><td> The ID of the last release of the project </td></tr>
    <tr><td>Octopus.Release.Previous.Number          </td><td> *1.2.2*                                  </td><td> The version number of the last release of the project </td></tr>
    <tr><td>Octopus.Release.PreviousForEnvironment.Id </td><td> *releases-122*                          </td><td> The ID of the last release of the project to the current environment </td></tr>
    <tr><td>Octopus.Release.PreviousForEnvironment.Number </td><td> *1.2.2*                             </td><td> The version number of the last release of the project to the current environment </td></tr>
    <tr><td>Octopus.Release.CurrentForEnvironment.Id </td><td> *releases-122*                           </td><td> The ID of the release of the last successful deployment to the current environment </td></tr>
    <tr><td>Octopus.Release.CurrentForEnvironment.Number </td><td> *1.2.2*                              </td><td> The version number of the release the last successful deployment to the current environment </td></tr>
    <tr><td>Octopus.Task.Argument[\_name\_]          </td><td> *deployments-123*                        </td><td> Argument values provided when creating the task </td></tr>
    <tr><td>Octopus.Task.Id                          </td><td> *servertasks-123*                        </td><td> The ID of the task                       </td></tr>
    <tr><td>Octopus.Task.Name                        </td><td> *Deploy release 1.2.3 to Production*     </td><td> The name of the task                     </td></tr>
    <tr><td>Octopus.Tentacle.CurrentDeployment.PackageFilePath </td><td> *C:\Octopus\Tentacle\Packages\OctoFx.1.2.3.nupkg* </td><td> The path to the package file being deployed </td></tr>
    <tr><td>Octopus.Tentacle.CurrentDeployment.TargetedRoles </td><td> *web-server*                             </td><td> The intersection of the roles targeted by the step, and those applied to the machine </td></tr>
    <tr><td>Octopus.Tentacle.PreviousInstallation.CustomInstallationDirectory </td><td> *C:\InetPub\WWWRoot\OctoFx*              </td><td> The directory into which the previous version of the package was deployed </td></tr>
    <tr><td>Octopus.Tentacle.PreviousInstallation.OriginalInstalledPath </td><td> *C:\Octopus\Tentacle\Apps\Production\OctoFx\1.2.2* </td><td> The directory into which the previous version of the package was extracted </td></tr>
    <tr><td>Octopus.Tentacle.PreviousInstallation.PackageFilePath </td><td> *C:\Octopus\Tentacle\Packages\OctoFx.1.2.2.nupkg* </td><td> The path to the package file previously deployed </td></tr>
    <tr><td>Octopus.Tentacle.PreviousInstallation.PackageVersion </td><td> *1.2.3*                                  </td><td> The previous version of the package that was deployed to the Tentacle </td></tr>
    <tr><td>Octopus.Web.DeploymentLink               </td><td> */app/deployment/deployments-123*        </td><td> A path relative to the Octopus Server URL at which the deployment can be viewed </td></tr>
    <tr><td>Octopus.Web.ProjectLink                  </td><td> */app/projects/projects-123*             </td><td> A path relative to the Octopus Server URL at which the project can be viewed </td></tr>
    <tr><td>Octopus.Web.ReleaseLink                  </td><td> */app/releases/releases-123*             </td><td> A path relative to the Octopus Server URL at which the release can be viewed </td></tr>
  </table>
</div>

## Action {#Systemvariables-Action}

Action-level variables are available during execution of an action. Indexer notion such as `Octopus.Action[Website].TargetRoles` can be used to refer to values for different actions.

| Name                                     | Example                                  | Description                              |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Octopus.Action.Id                        | *85287bef-fe6c-4eb7-beef-74f5e5a6b5b0*   | The ID of the action                     |
| Octopus.Action.IsSkipped                 | *True*                                   | Whether or not the action has been skipped in the current deployment *(Boolean)* |
| Octopus.Action.Manual.Instructions       | *Don't break anything :)*                | The instructions provided for a manual step |
| Octopus.Action.Manual.ResponsibleTeamIds | *teams-123,teams-124*                    | The teams responsible for completing a manual step*(List)* |
| Octopus.Action.MaxParallelism            | *5*                                      | The maximum number of machines on which the action will concurrently execute *(Number)* |
| Octopus.Action.Name                      | *Website*                                | The name of the action                   |
| Octopus.Action.Number                    | *5*                                      | The sequence number of the action in the deployment process *(Number)* |
| Octopus.Action.Package.CustomInstallationDirectory | *C:\InetPub\WWWRoot\OctoFx*              | If set, a specific directory to which the package will be copied after extraction |
| Octopus.Action.Package.CustomInstallationDirectoryShouldBePurgedBeforeDeployment | *False*                                  | If true, the all files in the `Octopus.Action.Package.CustomInstallationDirectory` will be deleted before deployment *(Boolean)* |
| Octopus.Action.Package.DownloadOnTentacle | *False*                                  | If true, the package will be downloaded by the Tentacle, rather than pushed by the Octopus server*(Boolean)* |
| Octopus.Action.Package.IgnoreConfigTransformationErrors | *False*                                  | If true, any errors in configuration transformations will be treated as informational rather than errors that will fail the deployment** ***(Boolean)* |
| Octopus.Action.Package.IgnoreVariableReplacementErrors | *False*                                  | If true, any errors in variable replacement will be treated as a warning rather than an error that will fail the deployment. (*Boolean*) |
| Octopus.Action.Package.NuGetFeedId       | *feeds-123*                              | The ID of the NuGet feed from which the package being deployed was pulled |
| Octopus.Action.Package.NuGetPackageId    | *OctoFx.RateService*                     | The ID of the NuGet package being deployed |
| Octopus.Action.Package.NuGetPackageVersion | *1.2.3*                                  | The version of the NuGet package being deployed |
| Octopus.Action.Package.SkipIfAlreadyInstalled | *False*                                  | If true, and the version of the package being deployed is already present on the machine, its re-deployment will be skipped (use with caution) *(Boolean)* |
| Octopus.Action.Package.Ssh.ApplicationsDirectoryPath | */home/user/.tentacle/apps/*             | The applications directory used for deployment on the target machine |
| Octopus.Action.Package.Ssh.PackageFileName | */home/user/.tentacle/packages/OctoFx.RateService.1.2.3.nupkg.tar.gz* | The package file being deployed on the target machine |
| Octopus.Action.Package.Ssh.PackagesDirectoryPath | */home/user/.tentacle/packages/*         | The packages directory used for deployment on the target machine |
| Octopus.Action.Package.Ssh.RootDirectoryPath | */home/user/.tentacle/*                  | The root directory used for deployment on the target machine |
| Octopus.Action.Package.Ssh.ToolsDirectoryPath | */home/user/.tentacle/tools/*            | The tools directory used for deployment on the target machine |
| Octopus.Action.Script.ScriptBody         | *Write-Host 'Hello!'*                    | The script being run in a script step    |
| Octopus.Action.Script.Syntax             | *PowerShell*                             | The syntax of the script being run in a script step |
| Octopus.Action.SkipRemainingConventions  | *True*                                   | If set by the user, completes processing of the action without runnning further conventions/scripts *(Boolean)* |
| Octopus.Action.TargetRoles               | *web-server,frontend*                    | Machine roles targeted by the action *(List)* |
| Octopus.Action.Template.Id               | *actiontemplates-123*                    | If the action is based on a step template, the ID of the template |
| Octopus.Action.Template.Version          | *123*                                    | If the action is based on a step template, the version of the template in use *(Number)* |
| Octopus.Action.Status.Error              | *The server could not be contacted*      | If the action failed because of an error, a description of the error |
| Octopus.Action.Status.ErrorDetail        | *System.Net.SocketException: The server ...* | If the action failed because of an error, a full description of the error |

## Azure {#Systemvariables-Azure}

| Name                                     | Example                                  | Description                              |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Octopus.Action.Azure.CertificateThumbprint | *86B5C8E5553981FED961769B2DA3028C619596AC* | The thumbprint of the X509 certificate used to authenticate with the Azure Subscription targeted by this action |
| Octopus.Action.Azure.PackageExtractionPath | Z:\Temp\packages\                        | If set by the user, the temporary path to extract Azure packages into during deployment |
| Octopus.Action.Azure.SubscriptionId      | *8affaa7d-3d74-427c-93c5-2d7f6a16e754*   | The Azure Subscription Id being targeted by this action |
| Octopus.Action.Azure.ResourceGroupDeploymentName | my-resourcegroupdeployment-name          | Override the auto-generated resource group deployment name when deploying a resource group |

## Azure Cloud Service {#Systemvariables-AzureCloudService}

| Name                                     | Example                                  | Description                              |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Octopus.Action.Azure.CloudServiceConfigurationFileRelativePath | *ServiceConfiguration.Custom.cscfg*      | If set by the user, the relative path to the \*.cscfg file, with a fallback to ServiceConfiguration.{Environment}.cscfg or ServiceConfiguration.Cloud.cscfg |
| Octopus.Action.Azure.CloudServiceName    | *my-cloudservice-web*                    | The name of the Cloud Service being targeted by this action |
| Octopus.Action.Azure.CloudServicePackageExtractionDisabled | True                                     | Octopus will not unpack the \*.cspkg file if this variable is set to True, instead the \*.cspkg file will be pushed to Azure as-is |
| Octopus.Action.Azure.CloudServicePackagePath | *Z:\Temp\packages\my-cloudservice-web.cspkg* | The path of the \*.cspkg file used by this action |
| Octopus.Action.Azure.LogExtractedCspkg   | *True*                                   | If true, the contents of the extracted \*.cspkg will be written to the log to help diagnose deployment issues *(Boolean)* |
| Octopus.Action.Azure.Slot                | *Staging* or *Production*                | The slot of the Cloud Service being targeted by this action |
| Octopus.Action.Azure.StorageAccountName  | *my-storage-account*                     | The name of the Azure Storage Account where \*.cspkg files will be uploaded to for deployment to the Cloud Service |
| Octopus.Action.Azure.SwapIfPossible      | *True*                                   | If true, the action will attempt to perform a VIP swap instead of deploying directly into the targeted Slot |
| Octopus.Action.Azure.UploadedPackageUri  | https://my-storage-account/container/my-cloudservice.web.cspkg | The Storage URI of the \*.cspkg file that will be deployed to the Cloud Service |
| Octopus.Action.Azure.UseCurrentInstanceCount | *True*                                   | If true, the action will maintain the number of Instances in the Cloud Service rather than reverting to what is defined in the \*.cspkg file |
| Octopus.Action.Azure.DeploymentLabel     | my custom label for build 3.x.x          | If set, the custom deployment label will be used for the Azure cloud service deployment. ***Introduced in Calamari version 3.4.1 which was released with Octopus Deploy version 3.4.4.*** |

## Azure Web Apps {#Systemvariables-AzureWebApps}

| Name                                     | Example                  | Description                              |
| ---------------------------------------- | ------------------------ | ---------------------------------------- |
| Octopus.Action.Azure.WebAppName          | *my-web-app*             | The name of the Web App being targeted by this deployment |
| Octopus.Action.Azure.WebSpaceName        | *SoutheastAsiaWebSpace1* | The name of the Web Space being targeted by this deployment |
| **Octopus.Action.Azure.RemoveAdditionalFiles** | *True*                   | When *True* instructs Web Deploy to delete files from the destination that aren't in the source package |
| **Octopus.Action.Azure.PreserveAppData** | *True*                   | When *True* instructs Web Deploy to skip Delete operations in the **App\_Data** directory |
| Octopus.Action.Azure.AppOffline          | *True*                   | When *True* instructs Web Deploy to safely bring down the app domain by adding a blank **app\_offline.html** file in the site root |

## Output {#Systemvariables-Output}

Output variables are collected during execution of a step and made available to subsequent steps using notation such as `Octopus.Action[Website].Output[WEBSVR01].Package.InstallationDirectoryPath`to refer to values base on the action and machine that produced them. See also [Output variables](/docs/deploying-applications/variables/output-variables.md).

| Name                                     | Example                                  | Description                              |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Octopus.Action[\_name\_].Output.\_property\_ | *Octopus.Action[Website].Output.WarmUpResponseTime* | The results of calling `Set-OctopusVariable` during an action are exposed for use in other actions using this pattern |
| Octopus.Action[\_name\_].Output.Manual.Notes | *Signed off by Alice*                    | Notes provided by the user who completed a manual step |
| Octopus.Action[\_name\_].Output.Package.InstallationDirectoryPath | *C:\Octopus\Tentacle\Apps\Production\MyApp\1.2.3* | The directory to which the package was installed |
| Octopus.Action[\_name\_].Output.Manual.ResponsibleUser.DisplayName | *Alice King*                             | The full name of the user who completed the manual step |
| Octopus.Action[\_name\_].Output.Manual.ResponsibleUser.EmailAddress | *[alice@example.com](mailto:alice@example.com)* | The email address of the user who completed the manual step |
| Octopus.Action[\_name\_].Output.Manual.ResponsibleUser.Id | *users-123*                              | The ID of the user who completed the manual step |
| Octopus.Action[\_name\_].Output.Manual.ResponsibleUser.Username | *alice*                                  | The username of the user who completed the manual step |

## Step {#Systemvariables-Step}

Step-level variables are available during execution of a step. Indexer notion such as `Octopus.Step[Website].Number` can be used to refer to values for different steps.

| Name                            | Example                                  | Description                              |
| ------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Octopus.Step.Id                 | *80b3ad09-eedf-40d6-9b66-cf97f5c0ffee*   | The ID of the step                       |
| Octopus.Step.Name               | *Website*                                | The name of the step                     |
| Octopus.Step.Number             | *2*                                      | The number of the step*(Number)*         |
| Octopus.Step.Status.Code        | *Succeeded*                              | A code describing the current status of the step |
| Octopus.Step.Status.Error       | *The server could not be contacted*      | If the step failed because of an error, a description of the error |
| Octopus.Step.Status.ErrorDetail | *System.Net.SocketException: The server could not be contacted (at ...)* | If the step failed because of an error, a full description of the error |

## Agent {#Systemvariables-Agent}

Agent-level variables describe the deployment agent or Tentacle on which the deployment is executing.

| Name                                     | Example                                  | Description                              |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Octopus.Tentacle.Agent.ApplicationDirectoryPath | *C:\Octopus\Tentacle\Apps*               | The directory under which the agent installs packages |
| Octopus.Tentacle.Agent.InstanceName      | *Tentacle*                               | The instance name that the agent runs under |
| Octopus.Tentacle.Agent.ProgramDirectoryPath | *C:\Program Files\Octopus Deploy\Tentacle* | The directory containing the agent's own executables |
| **Octopus.Agent.ProgramDirectoryPath**   | *C:\Program Files\Octopus Deploy\Octopus* | The directory containing either the server or tentacle's executables depending on which the step being executed on |

## Server {#Systemvariables-Server}

Server-level variables describe the Octopus server on which the deployment is running.

| Name                | Example                                  | Description                              |
| ------------------- | ---------------------------------------- | ---------------------------------------- |
| Octopus.Web.BaseUrl | *[https://my-octopus](https://my-octopus)* | The default URL at which the server can be accessed |

## Tracking deployment status {#Systemvariables-DeploymentStatusTrackingdeploymentstatus}

During deployment, Octopus provides variables describing the status of each step.

Where `S` is the step name, Octopus will set:

```powershell
Octopus.Step[S].Status.Code
Octopus.Step[S].Status.Error
Octopus.Step[S].Status.ErrorDetail
```

Status codes include `Pending`, `Skipped`, `Abandoned`, `Cancelled`, `Running`, `Succeeded` and `Failed`.

For an action `A:`

```powershell
Octopus.Action[A].IsSkipped
```

For the deployment as a whole:

```powershell
Octopus.Deployment.Error
Octopus.Deployment.ErrorDetail
```

:::hint
**Error detail returned**
Octopus.Deployment.Error and Octopus.Deployment.ErrorDetail will only display the exit code and Octopus stack trace for the error. As we cannot parse the deployment log, we can only extract the exit/error codes. It cannot show detailed information on what caused the error. For full information on what happened when the deployment fails, you will need to reference the logs.
:::

## User-modifiable settings {#Systemvariables-User-modifiablesettings}

The following variables can be defined as variables in your project to modify the way Octopus behaves.

| Name                                     | Example | Description                              |
| ---------------------------------------- | ------- | ---------------------------------------- |
| **Octopus.Acquire.MaxParallelism**       | 3       | Maximum number of NuGet packages that should be downloaded at once when acquiring packages. |
| **OctopusPrintVariables**                | True    | Set to "True" to tell Tentacle to print the value of all variables passed to it. We recommend only using this setting for non-production environments. |
| **OctopusPrintEvaluatedVariables**       | True    | Set to "True" to tell Tentacle to print the value of all variables passed to it after evaluating them. We recommend only using this setting for non-production environments. |
| **OctopusTreatWarningsAsErrors**         | True    | Set to "True" to have warnings from XML configuration transforms or PowerShell scripts treated as if they were errors, failing the deployment. |
| **OctopusSkipFreeDiskSpaceCheck**        | True    | Set to "True" to skip the check for available free disk space when deploying packages. ***Introduced in Calamari version 3.1.30 which was released with Octopus Deploy version 3.2.18.*** |
| **OctopusFreeDiskSpaceOverrideInMegaBytes** | 100     | The amount (in megabytes) of available free disk space we should check for (overriding the default 500MB), failing the deployment if not enough free disk space is available. ***Introduced in Calamari version 3.1.30 which was released with Octopus Deploy version 3.2.18.*** |
| **Octopus.Action.PowerShell.CustomPowerShellVersion** | 2.0     | If specified, your PowerShell scripts will be invoked using `PowerShell.exe -version {Version}` where {Version} is the value you specified. Accepted values are *2.0*, *3.0*, *4.0, 5.0* ***Introduced in Calamari version 3.3.13 which was released with Octopus Deploy version 3.3.13.*** |
| **OctopusDeleteScriptsOnCleanup**        | False   | For packaged scripts, set to "False" to keep the PreDeploy/Deploy/PostDeploy scripts in the target directory (i.e. don't cleanup). |
| **Octopus.Action.Script.SuppressEnvironmentLogging** | True    | To suppress/disable the environment logging that occurs from script (eg. PowerShell or Bash Script Environment Variables logging). This only suppresses script logging and does not suppress the Octopus or Calamari environment logging. ***Introduced in Calamari version 3.6.5 which was released with Octopus Deploy version 3.6.0.*** |
