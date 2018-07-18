---
title: Deregister from
description: Using the Tentacle.exe command line executable to deregister a deployment target from an Octopus Server.
---

Deregisters this deployment target from an Octopus Server

**Deregister from options**

```text
Usage: Tentacle deregister-from [<options>]

Where [<options>] is any of:
      --instance=VALUE       Name of the instance to use
      --server=VALUE         The Octopus Server - e.g., 'http://octopus'
      --apiKey=VALUE         Your API key; you can get this from the Octopus
                               web portal
  -u, --username=VALUE       If not using API keys, your username
  -p, --password=VALUE       In not using API keys, your password
  -m, --multiple             Deregister all machines that use the same thumbprint
 
Or one of the common options:
      --console              Don't attempt to run as a service, even if the
                               user is non-interactive
      --nologo               Don't print title or version information
      --noconsolelogging     Don't log to the console
```

