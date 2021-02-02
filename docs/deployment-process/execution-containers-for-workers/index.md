---
title: Execution containers for workers
description: How to set a step in your deployment/runbook process to run inside a container.
position: 15
---

For a [step](/docs/projects/steps/index.md) running on a [worker](docs/infrastructure/workers/index.md) or on the [Octopus Server](docs/infrastructure/workers/built-in-worker.md), you can select a Docker image to execute the step inside of.

When an execution container is configured for a step, Octopus will still connect to the worker machine via a [Tentacle or SSH](/docs/infrastructure/workers/index.md#register-a-worker-as-a-listening-tentacle). The difference is that the specified image will be run as a container and the step will be executed inside the container.

See the [blog post](https://octopus.com/blog/execution-containers) announcing this feature for some added context.

## Requirements
You need Docker installed and running on the [worker](docs/infrastructure/workers/index.md)/Octopus Server ([built-in worker](/docs/infrastructure/workers/built-in-worker.md)), in order to use execution containers for workers

### Octopus cloud dynamic worker pools 
[Octopus Cloud dynamic workers](/docs/infrastructure/workers/dynamic-worker-pools.md) have Docker pre-installed and support execution containers, with the exception of Windows 2016 pools. Dynamic worker pools with a VM type of Windows Server Core 2016 do not have Docker installed, and they cannot be used to run execution containers. 

![](images/hosted-worker-pools-execution-containers.png "width=500")


## How to use execution containers for workers 

- Configure a [feed](/docs/packaging-applications/package-repositories/docker-registries/index.md) in Octopus Deploy for a Docker registry.
  - [Add Docker Hub as an external feed](https://octopus.com/blog/build-a-real-world-docker-cicd-pipeline#add-docker-hub-as-an-external-feed).
- Add a project and define a deployment process (or add a [runbook](/docs/runbooks/index.md)).
- Set the **Execution Location** for your step to **Run on a worker**.
- In **Container Image** select **Runs on a worker inside a container**.
- Choose the previously added container registry.
- Enter the name of the image (aka execution container) you want your step to run in. (e.g. !docker-image <octopusdeploy/worker-tools:ubuntu.18.04>).
- Click **Save**.
- Click **Create release & deploy**.

![](images/selector.png "width=500")

## First deployment on a docker container

:::hint
Pre-pulling your chosen image will save you time during deployments.
:::

When you choose to run one or more of your deployment steps in a container, your deployment process will `docker pull` the image you provide at the start of each deployment during package acquisition.

For your first deployment this may take a while since your docker image won't be cached. You can pre-pull the desired docker image on your worker before your first deployment to avoid any delays.

## Which Docker images can I use? {#which-image} {#what-docker-image-should-i-use} <!--second anchor to avoid broken links to old header-->

:::hint
The easiest way to get started is to use the [worker-tools](#worker-tools-images) images built by Octopus Deploy.
:::

When a step is configured to use an execution container, [Calamari](/docs/octopus-rest-api/calamari.md) (the Octopus deployment utility) is executed inside the specified container.
Calamari is a .NET Core self-contained executable, and the Docker image will **need to include the dependencies required to execute a .NET self-contained executable**.  These dependencies can be found in the [.NET docs](https://docs.microsoft.com/en-us/dotnet/core/install/linux-ubuntu#dependencies). [Microsoft provides base images which include these dependencies](https://hub.docker.com/_/microsoft-dotnet-core-runtime-deps/).     

:::warning
Images based on Alpine Linux (or any distro using `musl` instead of `glibc`) can not currently be used as execution containers.
:::

## The octopusdeploy/worker-tools Docker images {#worker-tools-images} 

For convenience, we provide some images on Docker Hub [octopusdeploy/worker-tools](https://hub.docker.com/r/octopusdeploy/worker-tools) which include common tools used in deployments. 

:::hint
We recommend using our `worker-tools` image as a starting point for your own custom image to run on a worker
:::

The canonical source for what is contained in the `octopusdeploy/worker-tools` images is the `Dockerfile`'s in the [GitHub repo](https://github.com/OctopusDeploy/WorkerTools). For example: 
- The [Ubuntu 18.04 Dockerfile](https://github.com/OctopusDeploy/WorkerTools/blob/master/ubuntu.18.04/Dockerfile)
- The [Windows 2019 Dockerfile](https://github.com/OctopusDeploy/WorkerTools/blob/master/windows.ltsc2019/Dockerfile)

Some of the tools included are:

- Octopus Deploy CLI and .NET client library
- .NET Core
- Java (JDK)  
- NodeJS
- Azure CLI 
- AWS CLI 
- Google Cloud CLI
- kubectl 
- Helm
- Terraform
- Python
