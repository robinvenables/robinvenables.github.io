---
title: "Migrating From Terraform to OpenTofu"
date: 2025-05-05T06:00:00+03:00
draft: false
categories: ["devops"]
tags: ["iac","opentofu","terraform"]
summary: "I have been using Terraform to provision and maintain this domain's DNS configuration on ClouDNS. Although they are not all totally applicable to my situation, here are the reasons for using an Infrastructure as Code (IaC) tool:"
---

I have been using Terraform to provision and maintain this domain's DNS configuration on [ClouDNS][1]. Although they are not all totally applicable to my situation, here are the reasons you should be using an Infrastructure as Code (IaC) tool:

- Consistency and Repeatability - Manually provisioning infrastructure is error-prone. IaC ensures your environments (dev, staging, production) are built from the same source code, reducing configuration drift and human mistakes.

- Version Control and Auditability - IaC allows you to manage infrastructure definitions in Git (or any VCS). Every change is tracked, reviewable, and revertible, just like application code. This gives you a clear audit trail and improves team collaboration.

- Automation and Speed - Provisioning infrastructure becomes as simple as running a command or triggering a CI/CD pipeline. This drastically reduces setup time and supports fast, repeatable deployments.

- Scalability and Efficiency - IaC makes it easy to scale resources up or down, replicate environments, or spin up short-lived testing stacks without manual intervention, saving time and reducing cost.

- Disaster Recovery and Documentation - Your infrastructure is essentially self-documenting with IaC. If your environment is destroyed, you can rebuild it automatically from your codebase which is critical for disaster recovery and business continuity.

Recently, HashiCorp transitioned Terraform from its open-source MPL 2.0 license to the more restrictive Business Source License (BUSL). This change fundamentally alters the terms under which Terraform can be used and distributed, particularly in commercial contexts. The BUSL is not an open-source license. While it still allows source code access and usage for non-production and non-commercial purposes, it limits what others can do with the software in production. Open source is more than just access to code. It's about freedom: to use, modify, fork, and distribute without the looming possibility of legal or commercial restrictions.

[OpenTofu][2] is a fork of Terraform that remains fully open-source under the MPL 2.0 license. It’s overseen by the Linux Foundation and supported by a growing number of contributors and organizations that share the same open values. Migration from Terraform to OpenTofu was straightforward. OpenTofu is a drop-in replacement for Terraform 1.5.x, with the same HCL syntax, state file compatibility, and provider ecosystem.

If you care about keeping your infrastructure tooling open, auditable, and community-led, I recommend giving OpenTofu a serious look.

[1]: https://www.cloudns.net
[2]: https://opentofu.org