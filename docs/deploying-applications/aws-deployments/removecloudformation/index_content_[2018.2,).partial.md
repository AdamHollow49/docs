Octopus supports the deletion of an existing AWS CloudFormation stack through the `Delete a CloudFormation stack` step. This step deletes a CloudFormation stack using AWS credentials managed by Octopus.

The proceeding instructions can be followed to configure the `Delete a CloudFormation stack` step.

## Create an AWS Account

The instructions at [Creating an AWS Account](/docs/infrastructure/aws/creating-an-aws-account/index.md) detail the procedure for creating an account in Octopus.

## Create a AWS Account Project Variable

AWS accounts are included in a project through a project variable of the type `Amazon Web Services Account`.

![AWS Account Variable](aws-account-variable.png "width=500")

The `Add Variable` window is then displayed and lists all the AWS accounts, as well as an account called `Role Assigned to the AWS Instance Executing the Deployment`.

Select the account that was created in the previous step to assign it to the variable.

![AWS Account Variable Selection](aws-account-variable-selection.png "width=500")

## Add the CloudFormation Step

Add the `Delete a CloudFormation stack` step to the project, and provide it a name.

![Remove a CloudFormation stack Step](deploy-cloudformation-step.png "width=500")

### AWS Section

Select the variable that references the `Amazon Web Services Account` under the `AWS Account` section.

![AWS Account](step-aws-account.png "width=500")

The supplied account can optionally be used to assume a second role. This can be used to run the AWS commands with a role that limits the services that can be affected.

![AWS Role](step-aws-role.png "width=500")

### CloudFormation Section

Under the `CloudFormation` section, the AWS region and stack name need to be defined.

:::hint
If the stack does not exist, this step will succeed and not attempt to delete it again.
:::

You can also optionally wait for the stack to be deleted completely before finishing the step by selecting the `Wait for completion` checkbox.

:::hint
Unselecting the `Wait for completion` checkbox will allow the step to complete once that CloudFormation deletion has been initiated. However unselecting the option means that the step will not fail if the CloudFormation stack deletion fails.
:::

![AWS Region](step-aws-region.png "width=500")
