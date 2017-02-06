---
title: Creating environments
position: 0
---

[Octo.exe](/docs/api-and-integration/octo.exe-command-line/index.md) can be used to create environments on your Octopus instance.

```bash
octo create-environment [<options>]
```

Where `[<options>]` is any of:

**create-environment options**

```bash
Environment creation:
      --name=VALUE           The name of the environment
      --ignoreIfExists       If the project already exists, an error will be
                             returned. Set this flag to ignore the error.
Common options:
      --server=VALUE         The base URL for your Octopus server - e.g.,
                             http://your-octopus/
      --apiKey=VALUE         Your API key. Get this from the user profile
                             page.
      --user=VALUE           [Optional] Username to use when authenticating
                             with the server.
      --pass=VALUE           [Optional] Password to use when authenticating
                             with the server.
      --configFile=VALUE     [Optional] Text file of default values, with one
                             'key = value' per line.
      --debug                [Optional] Enable debug logging
      --ignoreSslErrors      [Optional] Set this flag if your Octopus server
                             uses HTTPS but the certificate is not trusted on
                             this machine. Any certificate errors will be
                             ignored. WARNING: this option may create a
                             security vulnerability.
      --enableServiceMessages
                             [Optional] Enable TeamCity service messages when
                             logging. 
```

## Basic example {#Creatingenvironments-Basicexample}

The following command will create an environment called *UAT*

```bash
Octo create-environment --name UAT --server http://MyOctopusServerURL.com --apikey MyAPIKey
```

:::success
**Tip**
Learn more about [Octo.exe](/docs/api-and-integration/octo.exe-command-line/index.md), and [creating API keys](/docs/how-to/how-to-create-an-api-key.md).
:::
