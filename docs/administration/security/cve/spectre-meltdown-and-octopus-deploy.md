---
title: Spectre (speculative execution side-channel vulnerabilities), meltdown, and Octopus Deploy
description: How the Spectre (speculative execution side-channel vulnerabilities) and meltdown vulnerabilities impact Octopus Deploy
position: 1
---

In January 2018 [Google announced](https://googleprojectzero.blogspot.com.au/2018/01/reading-privileged-memory-with-side.html) an attack that makes it practically possible to leak information from kernel memory on the host operating system.

> We have discovered that CPU data cache timing can be abused to efficiently leak information out of mis-speculated execution, leading to (at worst) arbitrary virtual memory read vulnerabilities across local security boundaries in various contexts.

So far, there are three known variants of the issue:

- Variant 1: bounds check bypass (CVE-2017-5753)
- Variant 2: branch target injection (CVE-2017-5715)
- Variant 3: rogue data cache load (CVE-2017-5754)

## Impact on Octopus Deploy

Octopus Deploy is not directly affected by these vulnerabilities. However, since the host operating system and underlying hardware can be vulnerable, any application running on affected systems can be affected. For your Octopus installation, this would include servers hosting:

- Octopus Server
- Microsoft SQL Server which is hosting your Octopus database
- The targets of your deployments

## Mitigation

Follow these security advisories from Microsoft to ensure your host operating system and underlying hardware are protected against these vulnerabilities:

- [Guidance to mitigate speculative execution side-channel vulnerabilities](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002)
- [Windows Client Guidance for IT Pros to protect against speculative execution side-channel vulnerabilities](https://support.microsoft.com/en-au/help/4073119/protect-against-speculative-execution-side-channel-vulnerabilities-in)