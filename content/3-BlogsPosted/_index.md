---
title: "Blogs Posted"
date: 2026-10-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

###  [Blog 1 - EC2 AMI TAG SHARING: SHARING EC2 AMI TAGS ACROSS AWS ACCOUNTS](3.1-Blog1/)
This blog introduces the EC2 AMI Tag Sharing feature, allowing AMI owners to share tags prefixed with `ec2:SharedTag/` natively alongside the AMI to other AWS accounts. This feature significantly streamlines the Golden AMI distribution pipeline, mitigates tag drift, and eliminates the complexity of maintaining intermediate EventBridge, SNS, and Lambda synchronization infrastructure.

###  [Blog 2 - AUTOMATING EC2 LICENSE MIGRATION FROM AWS MARKETPLACE TO BYOL](blog2/)
This article provides a theoretical and architectural analysis of migrating EC2 instances from AWS Marketplace License Included AMIs to the Bring Your Own License (BYOL) model using the `sample-ami-to-byol` automation tool. It dissects the decoupled cloud architecture of EC2 instances (Compute, Storage, Network, Identity), finite-state machine workflows, critical ordering dependencies, and Total Cost of Ownership (TCO) considerations.

###  [Blog 3 - ...](3.3-Blog3/)
This blog introduces the newly added session policies feature in Amazon EKS Pod Identity, which allows you to narrow IAM permissions flexibly and precisely for each pod without needing to create multiple separate IAM roles. This is an important step forward that helps apply the principle of least privilege more effectively in large-scale Kubernetes environments.