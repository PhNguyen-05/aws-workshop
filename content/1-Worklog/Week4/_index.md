---
title: "Week 4 Worklog"
date: 2026-10-26
weight: 4 
chapter: false
pre: " <b> 1.4. </b> "
---


### Week 4 Objectives:

* Understand Amazon EC2 (Elastic Compute Cloud) and its core features: instance types, AMIs, EBS, key pairs, security groups, Elastic IP.
* Prepare the networking infrastructure (VPC, security groups), then launch and connect to EC2 instances running Windows Server 2025 and Amazon Linux 2023.
* Practice basic EC2 operations: changing the instance type, EBS snapshots, creating and sharing a custom AMI, recovering access when a key pair is lost.
* Deploy the Node.js "AWS User Management" (CRUD) application on Linux (LAMP + Node.js) and Windows (XAMPP + Node.js).
* Apply IAM to manage costs and limit EC2 usage (by region, instance family, instance type, EBS volume type, IP address, and time period).
* Understand how to grant an application access to AWS services: using access keys (to be avoided) versus an IAM Role attached to EC2 (recommended).
* Clean up resources in the right order to avoid costs.


### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Learn the Amazon EC2 overview: <br>&emsp; + Concepts and use cases <br>&emsp; + Instance types, AMIs, storage options (EBS, instance store) <br>&emsp; + Security and access: key pairs, security groups, NACLs, IAM roles, Session Manager <br>&emsp; + Features: Auto Scaling, Load Balancing, Elastic IP, Placement Groups, multiple AZs <br> - **Practice (Preparation):** <br>&emsp; + Create a VPC for the Linux instance and a VPC for the Windows instance <br>&emsp; + Create a Security Group for Linux and one for Windows | 26/10/2026 | 26/10/2026 | <https://000004.awsfcaj.com/> <br> <https://000004.awsfcaj.com/1-introduce/> <br> <https://000004.awsfcaj.com/2-prerequiste/> |
| 3 | - **Practice:** <br>&emsp; + Launch a Microsoft Windows Server 2025 instance and connect from the computer (Remote Desktop) <br>&emsp; + Launch an Amazon Linux instance and connect (SSH) <br> - Take note: running instances keep incurring charges and must be terminated when no longer needed; remote access depends on the security group configuration | 27/10/2026 | 27/10/2026 | <https://000004.awsfcaj.com/3-launchwindowsinstance/> <br> <https://000004.awsfcaj.com/4-launchlinuxinstance/> |
| 4 | - **Practice Amazon EC2 Basic:** <br>&emsp; + Modify the instance type <br>&emsp; + Create and manage EBS snapshots <br>&emsp; + Create a Custom AMI and launch an instance from it <br>&emsp; + Recover access to a Windows instance (using Systems Manager) and a Linux instance (using user data) when the key pair is lost <br>&emsp; + Remote Desktop to EC2 Ubuntu <br>&emsp; + (Optional) EBS Snapshots Archive and Share AMI | 28/10/2026 | 28/10/2026 | <https://000004.awsfcaj.com/5-amazonec2basic/> |
| 5 | - **Deploy the AWS User Management application (Node.js, Express, Express-Handlebars, MySQL):** <br>&emsp; + Linux (Amazon Linux 2023): install LAMP (prepare, test, configure/secure the database, install phpMyAdmin), install Node.js, deploy the application <br>&emsp; + Windows (Windows Server 2025): install XAMPP, install Node.js, deploy the application <br>&emsp; + Open inbound ports in the security group (3000 for the application, 80 for XAMPP) and test the CRUD functions | 29/10/2026 | 29/10/2026 | <https://000004.awsfcaj.com/6-awsfcjmanagement-linux/> <br> <https://000004.awsfcaj.com/7-awsfcjmanagement-windows/> |
| 6 | - **Cost and usage governance with IAM:** restrict services by region; limit EC2 by instance family and instance type; manage EBS volume types; limit delete permissions by company IP and by time period <br> - **IAM Role for EC2 workshop:** create EC2 and an S3 bucket; use an IAM user's access key (not recommended); use an IAM role attached to EC2 (recommended) <br> - **Clean up** both workshops <br> - Summarize week 4 knowledge and write the worklog | 30/10/2026 | 30/10/2026 | <https://000004.awsfcaj.com/8-costusagegovernance/> <br> <https://000004.awsfcaj.com/9-cleanup/> <br> <https://000048.awsfcaj.com/> <br> <https://000048.awsfcaj.com/4-cleanup/> |


### Practice and learning details:

#### Lab 4: Introduction to Amazon EC2

**1. Amazon EC2 overview**

* Amazon EC2 provides on-demand, scalable compute capacity in the AWS Cloud, letting you launch virtual servers without upfront hardware investment. Use cases include web hosting, applications, databases, authentication services, and enterprise tasks.
* You can launch or stop any number of virtual servers, configure security and networking, manage storage, and scale capacity up or down.
* **Instance type:** defines the hardware configuration of the virtual server and must be chosen when launching an instance.
  * CPUs: Intel, AMD, AWS Graviton (ARM) and GPU; RAM varies by instance family; network performance ranges from moderate to 100+ Gbps.
  * Families: C (compute-optimized, e.g., batch processing), R (memory-optimized, e.g., databases), T and M (general-purpose).
* **AMI (Amazon Machine Image):** contains the operating system and configuration, application server and applications, launch permissions, and block device mapping. Options are AWS-provided AMIs, AWS Marketplace AMIs, and custom AMIs created from existing instances.
* **Storage:**
  * **EBS volume:** persistent, network-attached storage independent of the instance lifecycle.
  * **Instance store:** temporary storage physically attached to the host; data is lost when the instance is stopped or terminated.
* **Security and access:** key pairs (SSH), security groups (virtual firewalls), NACLs (stateless packet filtering at the subnet level), IAM roles (grant permissions to applications running on EC2), Systems Manager Session Manager (access without opening inbound SSH/RDP ports or distributing private keys).
* **Key features:**
  * Elastic infrastructure: EC2 Auto Scaling, Elastic Load Balancing, dynamic attach/detach of resources.
  * Networking: VPC, Elastic IP (static IPv4), Placement Groups.
  * High availability: multiple Availability Zones, Local Zones, AWS Outposts, Wavelength Zones.
  * Resource management: Tags, Resource Groups, CloudFormation.
  * Monitoring: CloudWatch, CloudTrail, Systems Manager.
* **Tips:** match the instance family to the workload to balance performance and cost; store data that must persist on EBS, not on instance store; associate Elastic IPs with running instances or release them when unused to avoid charges.
* The workshop was tested on Microsoft Windows Server 2025 and Amazon Linux 2023 (plus Ubuntu), using the sample Node.js application **AWS User Management** with CRUD functionality.

**2. Preparation**

* Before launching EC2, set up networking and security controls:
  * **VPC:** an isolated network for the instances; created separately for the Linux (2.1) and Windows (2.2) environments.
  * **Security Group:** a stateful virtual firewall controlling inbound and outbound traffic; created separately for Linux (2.3) and Windows (2.4).
* Note: a misconfigured VPC or security group can block legitimate traffic or expose instances. Security groups should allow only the traffic the application needs and block other access.

**3. Launch and connect to Windows Server 2025 / Amazon Linux**

* **Windows Server 2025:** Microsoft's newest server release with stronger security and better performance; AWS offers it through license-included AMIs, integrated with Systems Manager and Active Directory. Two parts: launching the instance (3.1) and connecting from the computer (3.2).
* **Amazon Linux:** the AWS-provided Linux distribution aimed at a secure, stable, high-performance environment for EC2 applications. Launching the instance (4.1) and connecting (4.2).
* Cost warning: running instances keep incurring charges, so terminate them when no longer needed. Security note: remote access (RDP/SSH) depends on a properly configured security group.

**4. Amazon EC2 Basic**

Basic operations with EC2 and its components, including changing configurations and recovering access when a key pair is lost:

* **5.1 Modify EC2 Instance Type:** change the instance type of an instance.
* **5.2 Create and Manage EBS Snapshots:** create and manage snapshots of EBS volumes.
* **5.3 Create Custom AMI:** create a custom AMI from an instance.
* **5.4 Launch an Instance from a Custom AMI:** launch a new instance from the created AMI.
* **5.5 Recovering Access to Windows Instances:** recover access to a Windows instance using AWS Systems Manager when the key pair is lost.
* **5.6 Recovering Access to Linux Instances:** recover access to a Linux instance using user data when the key pair is lost.
* **5.7 Remote Desktop to EC2-Ubuntu:** connect by remote desktop to an EC2 Ubuntu instance.
* **5.8 Amazon EBS Snapshots Archive (optional):** archive snapshots for long-term storage.
* **5.9 Share AMI (optional):** share an AMI.
* Security note: manage key pairs securely and know the alternative access methods. These operations help build resilient architectures and manage compute resources efficiently.

**5. Deploying the AWS User Management application on Amazon Linux 2023**

* The application uses Node.js, Express, Express-Handlebars and MySQL, and provides a CRUD interface to manage users.
* Prerequisites: an EC2 instance running Amazon Linux 2023, basic AWS and Linux knowledge, an AWS account with appropriate permissions.
* Steps:
  1. **Install LAMP (Linux, Apache, MySQL, PHP):** prepare the LAMP server (6.1.1), test the LAMP server (6.1.2), configure and secure the database server (6.1.3), install phpMyAdmin (6.1.4).
  2. **Install Node.js** (6.2).
  3. **Deploy the application** (6.3): clone the repository, install dependencies with npm, configure the database connection, start the server, and test the CRUD functions.
* Tip: when configuring MariaDB/MySQL, run `mysql_secure_installation` to set a strong root password and remove test databases. In production, use the latest LTS version of Node.js to get the most recent security patches.
* Warning: the EC2 security group must allow inbound traffic on the application's port (typically 3000) so it can be reached from a browser.

**6. Deploying Node.js applications on Amazon EC2 Windows**

* Deploy the same AWS User Management application on Windows Server 2025 Base.
* Steps:
  1. Install XAMPP (MySQL/MariaDB, Apache, PHP) as the database environment (7.1).
  2. Configure the database schema through phpMyAdmin.
  3. Install Node.js (7.2).
  4. Deploy and configure the application (7.3).
  5. Test the CRUD operations.
* Warning: the security group must allow inbound traffic on port 80 (XAMPP) and port 3000 (the Node.js app).
* Tip: XAMPP simplifies installing MySQL, Apache and PHP on Windows, which suits development and testing; for production, use Amazon RDS for MySQL instead of a local database for better scalability, reliability and security.

**7. Cost and usage governance with IAM**

* IAM offers fine-grained access control that supports both security and cost governance. EC2's flexibility makes it quick to deploy environments like XAMPP and LAMP, but that flexibility needs governance.
* Least-privilege IAM policies strengthen security and support cost optimization by preventing unauthorized resource provisioning; well-designed policies prevent both over-provisioning (waste) and under-provisioning (limited performance).
* Six practice topics:
  * **8.1** Restrict service usage by AWS Region.
  * **8.2** Limit EC2 usage by instance family.
  * **8.3** Control EC2 deployment by instance type.
  * **8.4** Manage the allowed EBS volume types.
  * **8.5** Limit permission to delete resources by the company's IP address.
  * **8.6** Limit permission to delete resources by time period.

**8. Clean up resources (EC2 workshop)**

In this order:
1. **Terminate EC2 instances:** EC2 → Instances → select the lab instances → Instance state → Terminate instance.
2. **Deregister AMIs:** EC2 → AMIs → select the created AMI → Actions → Deregister AMI.
3. **Delete EBS snapshots:** EC2 → Snapshots → select the lab snapshots → Actions → Delete snapshot.
4. **Delete Security Groups:** EC2 → Security Groups → Actions → Delete security group (make sure the group is not attached to any running instance).
5. **Delete Key Pairs:** EC2 → Key Pairs → select the lab key pair → Actions → Delete.
6. **Delete the VPC:** VPC → Your VPCs → select the lab VPC → Actions → Delete VPC (deleting the VPC also removes its subnets, route tables, internet gateways and NAT gateways).
7. **Delete IAM resources:** User Groups, Users, Policies, Roles.

#### Lab 48: Granting an Application Access to AWS Services with an IAM Role

**1. Objectives and architecture**

* The workshop covers two ways to give an application access to AWS services:
  1. **Access keys (access key ID and secret access key)** of an IAM user: how it works and why it should be avoided.
  2. **An IAM role attached to EC2:** the recommended approach.
* Services used: AWS IAM (user, access key, role), Amazon EC2 (hosts the application, the role attaches to it) and Amazon S3 (the bucket the application accesses).
* Scenario: a Python application running on EC2 uploads an object to an S3 bucket, first using an access key and then using an IAM role.

**2. Preparation**

* **1.1 Create EC2 Instance:** launch the EC2 instance that runs the application.
* **1.2 Create S3 bucket:** create the S3 bucket the application will access.

**3. Using an access key (not recommended)**

* **2.1 Generate IAM user and access key:** create an IAM user and generate its access key ID and secret access key.
* **2.2 Use access key:** use the access key in the Python application to upload an object to the S3 bucket.
* An access key is a long-term credential that has to be stored in the application/server, so it is easily exposed.

**4. Using an IAM role on EC2 (recommended)**

* **3.1 Create IAM role:** create an IAM role with permissions for the required service (the role in the lab is `EC2RoleS3Upload`).
* **3.2 Using IAM role:** attach the role to the EC2 instance and confirm the application can upload files to S3 without storing any key.

**5. Clean up (IAM Role workshop)**

* **Delete the S3 bucket** (region us-east-1): select the bucket `s3-instancerole-xxxx` → Empty → type `permanently delete` → Empty; then select the bucket again → Delete → enter the bucket name → Delete bucket.
* **Terminate the EC2 instance:** EC2 → Instances → select the lab instance → Instance state → Terminate instance → Terminate.
* **Delete the IAM user `IAMAccessKey`:** IAM → Users → select the user → Delete → choose to deactivate the access key → type `confirm` → Delete user.
* **Delete the IAM role `EC2RoleS3Upload`:** IAM → Roles → search `ec2` → select the role → Delete → enter the role name → Delete.


### Week 4 Achievements:

* Understood Amazon EC2 and its main components:
  * Instance types and families (C, R, T, M)
  * AMIs
  * EBS volumes and instance store
  * Key pairs, security groups, NACLs
  * Elastic IP and high-availability features (multiple AZs, Auto Scaling, Load Balancing)

* Learned the process of preparing a separate VPC and security group for the Linux and Windows environments before creating instances.

* Learned how to launch and connect to Windows Server 2025 and Amazon Linux 2023.

* Learned the basic EC2 operations: changing the instance type, EBS snapshots, Custom AMIs, launching from an AMI, sharing an AMI, EBS Snapshots Archive, and the ways to recover access when a key pair is lost.

* Understood how to deploy a Node.js application (Express, Express-Handlebars, MySQL) on Linux (LAMP + Node.js) and Windows (XAMPP + Node.js), and the ports to open in the security group (3000 and 80).

* Understood why production should use Amazon RDS for MySQL instead of a local database, and should use the LTS version of Node.js.

* Understood how to use IAM to manage costs: limits by region, instance family, instance type, EBS volume type, IP address, and time period.

* Distinguished the two ways of granting an application access to AWS services and understood why an IAM Role attached to EC2 is preferred over long-term access keys.

* Learned the EC2 resource clean-up order (instance → AMI → snapshot → security group → key pair → VPC → IAM) and how to clean up the IAM Role workshop resources (S3 bucket, EC2, IAM user, IAM role).
* ...
