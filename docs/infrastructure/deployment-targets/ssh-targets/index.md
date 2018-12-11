---
title: SSH Targets
description: Deploying software to Linux and Unix deployment targets.
position: 20
---

For Linux and Unix systems, you can configure Octopus Deploy to communicate with your deployment targets through SSH. [Calamari](/docs/infrastructure/deployment-targets/ssh-targets/Calamari-on-ssh-targets.md) is installed on the SSH deployment targets which contains all the information regarding conventions and deployments. The Octopus server communicates with the target through SSH and executes bash scripts to invoke Calamari. Calamari executes the scripts and the deployment target passes the task progress, logs, and artifacts back to the Octopus server.

Before you configure an SSH deployment target, review the [requirements](/docs/infrastructure/deployment-targets/ssh-targets/requirements.md) and ensure your systems have the required packages installed.

## Create an SSH Account

The SSH connection you configure will use an account with either an [SSH Key Pair]((/docs/infrastructure/accounts/ssh-key-pair.md) or a [Username and Password](/docs/infrastructure/accounts/username-and-password.md) that has access to the remote host. See [accounts](/docs/infrastructure/accounts/index.md) for instructions to configure the account.

## Add an SSH Connection

1. In the **Octopus Web Portal**, navigate to the **Infrastructure** tab, select **Deployment Targets** and click **{{ADD DEPLOYMENT TARGET,SSH CONNECTION}}**.
2. Click **ADD** on the SSH Connection card.
3. Enter the DNS or IP address of the deployment target, i.e., `example.com` or `10.0.1.23`.
4. Enter the port (port 22 by default) and click **NEXT**.

Make sure the target server is accessible by the port you specify.

The Octopus server will attempt to perform the required protocol handshakes and obtain the remote endpoint's public key fingerprint automatically rather than have you enter it manually. This fingerprint is stored and verified by the server on all subsequent connections.

If this discovery process is not successful, you will need to click **ENTER DETAILS MANUALLY**.

5. Give the target a name.
6. Select which environment the deployment target will be assigned to.
7. Choose or create at least one target role for the deployment target and click **Save**. Learn about [target roles](/docs/infrastructure/deployment-targets/target-roles/index.md).
8. Select the [account](/docs/infrastructure/accounts/index.md) that will be used for the Octopus server and the SSH target to communicate.
9. If entering the details manually, enter the **Host**, **Port** and the host's [fingerprint](#fingerprint).

You can retrieve the fingerprint of the default key configured in your sshd\_config file from the target server with the following command:

```bash
ssh-keygen -E md5 -lf /etc/ssh/ssh_host_rsa_key.pub | cut -d' ' -f2 | awk '{ print $1}' | cut -d':' -f2-
```

10. Specify whether Mono is installed on the SSH target or not to determine which version of [Calamari](/docs/api-and-integration/calamari.md) will be installed.
  - If Mono is installed a version of [Calamari built against the full .NET framework](/docs/infrastructure/deployment-targets/ssh-targets/calamari-on-ssh-targets.md#mono-calamari) will be installed.
  - If Mono is not installed a [self-contained version of Calamari](/docs/infrastructure/deployment-targets/ssh-targets/calamari-on-ssh-targets.md#self-contained-calamari) built against .NET Core will be installed on the target.
11. Click **Save**.

## Health Check

Once the target is configured, Octopus will perform an initial health check. Health checks are done periodically or on demand and ensure the endpoint is reachable, configured correctly and the required dependencies are are available (e.g. tar, see [requirements](/docs/infrastructure/deployment-targets/ssh-targets/requirements.md)), and ready to perform deployment tasks.

If Calamari is not present or is out-of-date, a warning will be displayed, however, Calamari will be updated when it is next required by a task.

If the SSH target is healthy, the version that is displayed is the version of the Octopus server instance.

If the fingerprint changes after initial configuration, the next health check will update the finger print. If the fingerprint returned during the handshake is different to the value stored in the database, the new fingerprint will show up in the logs. If you aren't expecting a change and you see this error it could mean you have been compromised!

## Running Scripts on SSH Endpoints

The introduction of [Raw Scripting](/docs/deployment-examples/custom-scripts/raw-scripting.md) provides the ability to run scripts on SSH endpoints without any additional Octopus dependencies. To facilitate these targets, [machine policies](/docs/infrastructure/machine-policies.md) allow you to configure health checks to test only for SSH connectivity to your machine to be considered healthy.
