---
title: "Migrating From Terraform to OpenTofu"
date: 2025-07-19T00:00:00+03:00
draft: false
categories: ["devops"]
tags: ["iac","opentofu","terraform"]
summary: "I have been using Terraform to provision and maintain this domain's DNS configuration. Recently, HashiCorp transitioned Terraform from its open-source MPL 2.0 license to the more restrictive Business Source License (BUSL). This change fundamentally alters the terms under which Terraform can be used and distributed, particularly in commercial contexts."
---

I have been using Terraform to provision and maintain this domain's DNS configuration. Recently, HashiCorp transitioned Terraform from its open-source MPL 2.0 license to the more restrictive Business Source License (BUSL). This change fundamentally alters the terms under which Terraform can be used and distributed, particularly in commercial contexts.

The BUSL is not an open-source license. While it still allows source code access and usage for non-production and non-commercial purposes, it limits what others can do with the software in production. Open source is more than just access to code. It's about freedom: to use, modify, fork, and distribute without the looming possibility of legal or commercial restrictions.

[OpenTofu][1] is a fork of Terraform that remains fully open-source under the MPL 2.0 license. It’s overseen by the [Linux Foundation][2] and supported by a growing number of contributors and organizations that share the same open values. Migration from Terraform to OpenTofu was straightforward. OpenTofu is a drop-in replacement for Terraform 1.5.x, with the same HCL syntax, state file compatibility, and provider ecosystem.

If you care about keeping your infrastructure tooling open, auditable, and community-led, I recommend giving OpenTofu a serious look.

[1]: https://opentofu.org
[2]: https://www.linuxfoundation.org