---
title: "Week 3 Worklog"
date: 2026-10-19
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Understand the role of AWS IAM (Identity and Access Management) in access control and know its main components: User, Group, Policy, Role.
* Practice creating IAM Groups, IAM Users, IAM Roles, and switching roles following the principle of least privilege.
* Understand Amazon VPC: CIDR, subnets, route tables, Internet Gateway, NAT Gateway.
* Distinguish Security Groups from Network ACLs, and understand VPC Flow Logs and Reachability Analyzer.
* Grasp the architecture of a multi-AZ VPC deployment with EC2, NAT Gateway, Session Manager, and CloudWatch monitoring.
* Understand the AWS Site-to-Site VPN model (Virtual Private Gateway, Customer Gateway, tunnels) and know how to clean up resources in the right order to avoid costs.


### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Learn the overview of AWS IAM: <br>&emsp; + Root user and why it should not be used for daily tasks <br>&emsp; + Features: shared account access, granular access control, secure application access <br>&emsp; + Security features: MFA, identity federation, CloudTrail integration <br>&emsp; + Access methods: Console, CLI, SDK/API <br> - Learn the 4 components: IAM User, IAM Group, IAM Policy, IAM Role <br> - Note that IAM and AWS STS are free | 19/10/2026 | 19/10/2026 | <https://000002.awsfcaj.com/> <br> <https://000002.awsfcaj.com/1-introduction/> |
| 3 | - **Practice:** <br>&emsp; + Create an IAM Group with administrative permissions <br>&emsp; + Create an IAM User and add it to the admin group <br>&emsp; + Set up secure login credentials and sign in as the Admin User to verify permissions <br> - Remember the principle: manage permissions through groups instead of attaching policies directly to users | 20/10/2026 | 20/10/2026 | <https://000002.awsfcaj.com/2-create-admin-user-and-group/> |
| 4 | - **Practice:** <br>&emsp; + Create the IAM Role `Admin` with the `AdministratorAccess` managed policy <br>&emsp; + Create the IAM User `OperatorUser` <br>&emsp; + Allow `OperatorUser` to assume the role <br>&emsp; + Sign in to the Console as `OperatorUser` and perform Switch Role <br>&emsp; + Switch back to the original role when done <br>&emsp; + **Clean up:** delete the user and group, check the IAM Dashboard | 21/10/2026 | 21/10/2026 | <https://000002.awsfcaj.com/3-aws-role/> <br> <https://000002.awsfcaj.com/4-switch-roles/> <br> <https://000002.awsfcaj.com/5-cleanup/> |
| 5 | - Learn Amazon VPC: <br>&emsp; + VPC concepts, CIDR `/16` to `/28`, secondary CIDR, IPv6, IPAM <br>&emsp; + Subnets (public, private, isolated), Route Table, Internet Gateway, NAT Gateway <br> - Learn firewalls in VPC: <br>&emsp; + Security Group (stateful, instance/ENI level) <br>&emsp; + Network ACL (stateless, subnet level, rules evaluated in numerical order) <br> - Learn about VPC Flow Logs and VPC Reachability Analyzer | 22/10/2026 | 22/10/2026 | <https://000003.awsfcaj.com/> <br> <https://000003.awsfcaj.com/1-introduce/> <br> <https://000003.awsfcaj.com/2-firewallinvpc/> |
| 6 | - Learn the process of building a multi-AZ VPC: create VPC, subnets, Internet Gateway, route tables, security groups, enable VPC Flow Logs <br> - Learn how to deploy EC2 and related components: NAT Gateway, Reachability Analyzer, EC2 Instance Connect Endpoint, Systems Manager Session Manager, CloudWatch monitoring <br> - Learn AWS Site-to-Site VPN: Virtual Private Gateway, Customer Gateway, BGP/static routing, 2 IPsec tunnels <br> - Learn the resource clean-up order and Infrastructure as Code templates (CloudFormation, CDK, Terraform) <br> - Summarize week 3 knowledge and write the worklog | 23/10/2026 | 23/10/2026 | <https://000003.awsfcaj.com/3-prerequisite/> <br> <https://000003.awsfcaj.com/4-createec2server/> <br> <https://000003.awsfcaj.com/5-vpnsitetosite/> <br> <https://000003.awsfcaj.com/6-cleanup/> <br> <https://000003.awsfcaj.com/7-infrastructureascode/> |


### Practice and learning details:

#### Lab 2: Managing Access Control with AWS IAM

**1. IAM overview**

* IAM is a security service for centrally managing access to AWS services and resources: creating user identities, fine-grained permissions, and secure authentication.
* The **root user** has full access to everything in the account: protect it with a strong password and MFA, do not use it for daily tasks, and create IAM users with appropriate permissions instead; use root only for specific tasks.
* Three core features:
  * Shared account access without sharing credentials.
  * Granular access control per user and per resource, following least privilege and custom policies.
  * Secure application access: manage credentials for applications on EC2 and use IAM roles for service-to-service communication.
* Security features: MFA, identity federation with external providers, CloudTrail integration for auditing, PCI DSS compliance support, eventual consistency model.
* Three access methods: AWS Management Console, command line tools (AWS CLI, PowerShell), programmatic access (AWS SDKs, IAM Query API over HTTPS). AWS CloudShell provides command-line access from the console with no local install.
* Cost: IAM and AWS STS are free; you only pay for the other AWS services you use.
* Four core components:
  * **IAM User:** an individual identity with long-term credentials.
  * **IAM Group:** a collection of users.
  * **IAM Policy:** permission definitions in JSON.
  * **IAM Role:** temporary credentials for service-to-service access or for trusted entities.

**2. Create IAM Group and IAM User**

* Create an IAM Group and configure administrative permissions.
* Create a new IAM User and add it to the administrative group.
* Set up secure login credentials and sign in to the AWS account as the Admin User to verify administrative access.
* Notes: document and securely store the credentials since later labs need them; manage permissions through groups rather than attaching policies to individual users.

**3. Create IAM Role and IAM User**

* Roles provide temporary credentials instead of long-term access keys and delegate permissions to trusted entities (IAM users in your account or AWS services). Using roles rather than direct policy attachments gives better control and simpler management.
* Objectives:
  * Create an IAM Role named **Admin** with the **AdministratorAccess** managed policy.
  * Create an IAM User named **OperatorUser**.
  * Configure role-assumption permissions for OperatorUser.
* Warning: `AdministratorAccess` grants full access; follow your organization's security policies when assigning administrative permissions.

**4. Switch Role**

* Switching roles means temporarily assuming a previously created role, including across AWS accounts, using temporary credentials instead of long-term keys, which fits least privilege.
* Steps: (1) configure role-switch permissions for OperatorUser, (2) access the AWS Console as OperatorUser, (3) perform the role switch.
* Prerequisites: completed the admin group/user section and the Admin role/OperatorUser section.
* Tip: after finishing work in the assumed role, switch back to your original role to keep audit trails clear and prevent unintended resource access.

**5. Clean up resources**

* Open the IAM Console, choose **Users**, select the lab user, choose **Delete**, and confirm in the dialog.
* Choose **User groups**, select the lab group, choose **Delete**, and confirm.
* Check the IAM Dashboard to confirm all lab resources are gone.

**General security tips:** use IAM Roles for temporary permissions instead of direct assignments, set session timeouts, monitor access activity through AWS CloudTrail, and regularly review permissions following least privilege.

#### Lab 3: Amazon VPC and AWS Site-to-Site VPN

**1. Introduction to Amazon VPC**

* A VPC is a logically isolated virtual network in AWS where you launch EC2, ECS/EKS, RDS and similar resources while controlling the network configuration; it is built on the AWS Nitro System.
* Key capabilities:
  * IP management: primary and secondary IPv4 CIDRs, native IPv6, Amazon VPC IP Address Manager (IPAM).
  * Segmentation: public, private, isolated and IPv6-only subnets across multiple AZs.
  * Routing and gateways: route tables, Internet Gateway, Egress-Only Internet Gateway (IPv6), NAT Gateway.
  * Hybrid and multi-VPC connectivity: Site-to-Site VPN, Direct Connect, VPC Peering, Transit Gateway, Cloud WAN.
  * Security: Security Groups (stateful), Network ACLs (stateless), AWS Network Firewall.
* IP addressing:
  * IPv4 CIDR block sizes range from `/16` (65,536 addresses) to `/28` (16 addresses); the primary CIDR cannot be modified after creation; up to 4 secondary CIDR blocks can be added.
  * VPC CIDRs must not overlap with on-premises subnets, peered VPCs or transit networks.
  * The default VPC is `172.31.0.0/16`, with a public subnet in each AZ.
  * IPv6 allocation is `/56` per VPC and `/64` per subnet.
  * Cost note: since 1 Feb 2024, AWS charges $0.005 per hour for every public IPv4 address.
* Use custom VPCs rather than the default VPC in production to apply least privilege, build multi-tier subnets (Public, Application, Database) and avoid CIDR conflicts.
* Observability components: VPC Flow Logs (capture IP traffic metadata, delivered to CloudWatch Logs, S3 or Kinesis Data Firehose) and VPC Reachability Analyzer (verifies paths between resources without sending packets).

**2. Firewalls in VPC**

* **Security Group:**
  * A virtual firewall at the instance (ENI) level, stateful: return traffic is automatically allowed.
  * Each instance can have up to 5 Security Groups.
  * By default all outbound traffic is allowed and all inbound is denied.
  * Rules can reference other Security Groups, AWS services or IP ranges.
* **Network ACL (NACL):**
  * An optional subnet-level firewall, stateless: explicit rules must be defined for both inbound and outbound traffic.
  * Each subnet is associated with exactly one NACL (default or custom); rules are evaluated in numerical order, lowest number first.
  * The default NACL allows all inbound and outbound traffic.
* The two layers complement each other: Security Groups give fine-grained instance-level control while NACLs give broader subnet-level protection.

**3. Preparation Steps**

* Goal: build a multi-AZ VPC with public and private subnets following AWS best practices.
* Order: create VPC → create Subnets → create Internet Gateway → create Route Tables → create Security Groups → enable VPC Flow Logs (the VPC Resource Map can be used to view the structure).
* Cost note: most VPC components are free but NAT Gateways incur charges. The workshop creates each component individually (instead of using the VPC Wizard) to show how they fit together.

**4. Deploying Amazon EC2 Instances**

* Multi-AZ architecture, highly available NAT Gateways, secure access (SSH and Session Manager), CloudWatch monitoring and alerting, and VPC Reachability Analyzer.
* Order: create EC2 server → test connection → create NAT Gateway → use Reachability Analyzer → create EC2 Instance Connect Endpoint (optional) → AWS Systems Manager Session Manager → CloudWatch Monitoring & Alerting. The prerequisite is having enabled VPC Flow Logs.
* Resources used in the lab: EC2 Public, EC2 Private, NAT-Gateway (NAT-Gateway-AZ1a, NAT-Gateway-AZ1b), and the VPC endpoints for Session Manager: SSM-Endpoint, SSM Messages Endpoint, EC2 Messages Endpoint.

**5. Setting up AWS Site-to-Site VPN**

* Site-to-Site VPN connects an on-premises data center to Amazon VPC.
* Concepts:
  * **Virtual Private Gateway (VPG):** the AWS-side tunnel termination point.
  * **Customer Gateway (CGW):** represents the customer's VPN device (hardware or software).
  * **BGP:** dynamic routing if the CGW supports it; otherwise use **static routing** and enter the routes yourself.
  * **IPsec:** the protocol creating the secure tunnel over the internet.
  * Each VPN connection has **2 tunnels** for high availability; connections are initiated from the CGW to the VPG.
* Steps: set up the VPG → set up the CGW → choose BGP or static routing → the tunnel comes up when traffic flows.
* Lab scenario: the main office is the VPC **ASG** (`10.10.0.0/16`) and the branch office is the VPC **ASG VPN** (`10.11.0.0/16`), in different Availability Zones. Initially both sides allow external SSH but the instances cannot ping each other by private IP; the goal is private-IP connectivity across the VPN.
* The customer is responsible for preparing the internet connection.

**6. Clean up resources**

* Delete resources in dependency order to avoid errors and stop charges:
  1. **EC2 instances:** EC2 Public, EC2 Private, EC2 Customer Gateway (if created) → Instance state → Terminate instance.
  2. **NAT Gateways:** NAT-Gateway, NAT-Gateway-AZ1a, NAT-Gateway-AZ1b → Actions → Delete NAT gateway, type `delete`; wait for the Deleted status (about 5-10 minutes).
  3. **Elastic IPs:** Actions → Release Elastic IP addresses.
  4. **VPC endpoints:** SSM-Endpoint, SSM Messages Endpoint, EC2 Messages Endpoint → Actions → Delete VPC endpoints.
  5. **VPN resources (if applicable):** delete the Site-to-Site VPN connection (wait 2-5 minutes); detach then delete the Virtual Private Gateway; delete the Customer Gateway.
  6. **VPCs:** delete ASG VPN first, then ASG → Actions → Delete VPC, type `delete`.
* Approximate costs: NAT Gateway $0.045/hour plus $0.045/GB processed; unused Elastic IP $0.005/hour; VPN connection $0.05/hour.
* Notes: instance store data is lost on termination; EBS volumes may remain if "Delete on Termination" is off; CloudWatch Logs, EBS snapshots and S3 buckets may still incur charges and must be deleted separately.
* Verify with the Billing Dashboard, Cost Explorer and Bills, and set Budget Alerts.

**7. Infrastructure as Code**

* IaC provides repeatability, version control, automation, standardization, uniform security policy, and audit trails.
* The workshop provides 3 templates that create a multi-AZ `10.10.0.0/16` VPC with public/private subnets, 2 NAT Gateways, route tables, security groups, SSM VPC endpoints, and VPC Flow Logs:
  * **CloudFormation (YAML):** parameters `VpcCidr` and `Environment` (dev, staging, prod); deployed with `aws cloudformation create-stack`.
  * **AWS CDK (TypeScript):** stack `ProductionVpcStack`; deployed with `cdk bootstrap` and `cdk deploy`.
  * **Terraform:** file `terraform/main.tf`; deployed with `terraform init`, `plan`, `apply`.
* Best practices: do not open SSH from `0.0.0.0/0` in production, use Secrets Manager or Parameter Store for secrets, least-privilege IAM, encryption, multi-AZ deployment, CI/CD, and separate dev/staging/prod environments.


### Week 3 Achievements:

* Understood the role of IAM and distinguished its 4 components:
  * IAM User
  * IAM Group
  * IAM Policy
  * IAM Role

* Understood why the root user should not be used for daily tasks and why MFA should be enabled.

* Learned the permission management workflow: create an admin group, create a user, assign permissions through the group; create the `Admin` role with `AdministratorAccess`, create `OperatorUser`, and use Switch Role to assume temporary permissions.

* Understood the principle of least privilege, the preference for roles with temporary credentials, and how to clean up users/groups after practice.

* Understood the Amazon VPC structure: CIDR, public/private/isolated subnets, route tables, Internet Gateway, NAT Gateway, IPv6 and IPAM.

* Distinguished Security Groups (stateful, instance level) from Network ACLs (stateless, subnet level, rules evaluated in order).

* Understood the role of VPC Flow Logs and VPC Reachability Analyzer in network monitoring and troubleshooting.

* Learned the process of building a multi-AZ VPC and deploying EC2 with NAT Gateway, Session Manager, and CloudWatch.

* Understood the AWS Site-to-Site VPN model: VPG, CGW, BGP/static routing, 2 IPsec tunnels.

* Learned the VPC resource clean-up order (EC2 → NAT Gateway → Elastic IP → VPC endpoint → VPN → VPC) and the costs that accumulate easily, such as NAT Gateways, Elastic IPs, public IPv4 addresses, and VPN connections.

* Became aware of Infrastructure as Code templates (CloudFormation, CDK, Terraform) for automated VPC deployment.
* ...
