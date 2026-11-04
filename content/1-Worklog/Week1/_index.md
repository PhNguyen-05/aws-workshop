---
title: "Week 1 Worklog"
date: 2026-10-05
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Week 1 Objectives:

* Connect and get acquainted with members of First Cloud AI Journey, and understand the company's rules and regulations.
* Understand the basic AWS service groups: Compute, Storage, Networking, Database.
* Create an AWS Free Tier account and become familiar with the AWS Management Console and AWS CLI.
* Complete the 5 tasks of the AWS Free Tier program (EC2, Amazon Bedrock, AWS Budgets, AWS Lambda, Amazon RDS) to gain hands-on experience with core services.
* Build the habit of managing costs and cleaning up resources after practice.


### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Meet and get acquainted with FCAJ members <br> - Read and take note of the internship unit's rules and regulations <br> - Learn about the study roadmap and reporting format (weekly worklog) <br> - Prepare the working environment: email, note-taking tools, GitHub repo for the report | 05/10/2026 | 05/10/2026 | |
| 3 | - Learn about AWS and cloud computing: <br>&emsp; + Cloud Computing concepts, pay-as-you-go pricing model <br>&emsp; + Global infrastructure: Region, Availability Zone, Edge Location <br> - Learn about the service groups: <br>&emsp; + Compute (EC2, Lambda) <br>&emsp; + Storage (S3, EBS) <br>&emsp; + Networking (VPC) <br>&emsp; + Database (RDS, DynamoDB) <br> - Learn about the Shared Responsibility Model | 06/10/2026 | 06/10/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Create an AWS Free Tier account (sign up, verification, choose a support plan) <br> - Learn about AWS Management Console and AWS CLI <br> - **Practice:** <br>&emsp; + Create an AWS account and secure it <br>&emsp; + Install & configure AWS CLI (`aws configure`) <br>&emsp; + Use AWS CLI to check account information, list regions, and view the EC2 service <br>&emsp; + **Task 3:** set up a cost budget with AWS Budgets and receive email alerts | 07/10/2026 | 07/10/2026 | <https://cloudjourney.awsstudygroup.com/> <br> <https://000001.awsfcaj.com/> |
| 5 | - Learn basic EC2: <br>&emsp; + Instance types (General purpose, Compute/Memory optimized, ...) <br>&emsp; + AMI <br>&emsp; + EBS and volume types <br>&emsp; + Key pair, Security group <br> - SSH connection methods to EC2 (SSH client, EC2 Instance Connect, Session Manager) <br> - Learn about Elastic IP and the difference between a regular public IP and an Elastic IP <br> - Read the notes on services that can quickly drain credits | 08/10/2026 | 08/10/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - **Practice:** <br>&emsp; + **Task 1 (EC2):** launch an instance named `Test Instance`, create the key pair `first-kp` (RSA, .pem), create a security group, connect via SSH, attach an EBS volume, then terminate the instance <br>&emsp; + **Task 2 (Bedrock):** submit the use case, select Claude Haiku 5.5, run a test prompt in the Playground <br>&emsp; + **Task 4 (Lambda):** create the function `http-function-url-tutorial` from the "Getting started with Lambda HTTP" blueprint, check the function URL, then delete the function <br>&emsp; + **Task 5 (RDS):** create an Aurora (PostgreSQL Compatible) database with Easy create, wait for the Available status, then delete the instance and cluster <br> - Review all resources to make sure everything has been cleaned up <br> - Check the Credits page in Billing to confirm the credits received | 09/10/2026 | 09/10/2026 | <https://000001.awsfcaj.com/4-h%C6%B0%E1%BB%9Bng-d%E1%BA%ABn-chi-ti%E1%BA%BFt-5-nhi%E1%BB%87m-v%E1%BB%A5-ki%E1%BA%BFm-ti%E1%BB%81n/> |

### Practice task details:

#### Task 1: Launch an EC2 instance

* Open the task "Launch an instance using EC2" in the **Explore AWS** widget on the Console Home, then choose **Start activity**.
* Name the instance, choose an AMI, and keep the default hardware configuration.
* Create a new key pair (name `first-kp`, type RSA, format `.pem`) and store the key file safely for later SSH use.
* Create a new security group with the default options, review the configuration, then choose **Launch instance**.
* After completing the task, **terminate** the instance to avoid incurring costs.

#### Task 2: Amazon Bedrock Playground

* Select the task "Use a foundation model in the Amazon Bedrock", choose the **Claude Haiku 5.5** model, and fill in the use case details.
* If you get an error about not having access to the model, create a support case (Account and billing → Account Activation → Bedrock Allowlisting) with your Account ID and Region, then wait for AWS to respond.
* Once access is granted, enter a test prompt, run it, and review the model's response.

#### Task 3: AWS Budgets

* Select the task "Set up a cost budget using AWS Budgets".
* Enter the email to receive notifications, create the budget, and verify it was created successfully.
* This is an important step for tracking costs and avoiding exceeding the credit limit.

#### Task 4: Web app with AWS Lambda

* Select the task "Create a web app using AWS Lambda" and use the **Getting started with Lambda HTTP** blueprint.
* Name the function `http-function-url-tutorial`, accept the acknowledgment, and create the function.
* After finishing, delete the function (enter `confirm` to confirm).

#### Task 5: Database with Amazon RDS

* Select the task "Create an Amazon RDS Database" and use **Easy create** with the **Aurora (PostgreSQL Compatible)** engine.
* Wait for the database to reach the **Available** status.
* Clean up: delete the DB instance (`database-1-instance-1`), then delete the DB cluster (`database-1`), uncheck creating a final snapshot, and enter `delete me` to confirm.


### Week 1 Achievements:

* Understood what AWS is, the pay-as-you-go model, and the basic service groups:
  * Compute
  * Storage
  * Networking
  * Database
  * ...

* Understood the structure of AWS global infrastructure (Region, Availability Zone, Edge Location) and the Shared Responsibility Model.

* Successfully created and configured an AWS Free Tier account.

* Became familiar with the AWS Management Console and learned how to find, access, and use services via the web interface.

* Installed and configured AWS CLI on the computer, including:
  * Access Key
  * Secret Key
  * Default Region
  * ...

* Used AWS CLI to perform basic operations such as:
  * Check account & configuration information
  * Retrieve the list of regions
  * View EC2 service
  * Create and manage key pairs
  * Check information about running services
  * ...

* Gained solid foundational knowledge of EC2: instance types, AMI, EBS, key pair, security group, Elastic IP, and SSH connection methods.

* Completed the practice tasks of the AWS Free Tier program:
  * EC2: launched an instance, connected via SSH, attached an EBS volume, and terminated it after use.
  * Amazon Bedrock: experimented with a foundation model (Claude Haiku 5.5) in the Playground.
  * AWS Budgets: set up a cost budget with email alerts.
  * AWS Lambda: created a web app from the HTTP blueprint, then deleted the function.
  * Amazon RDS: created an Aurora (PostgreSQL Compatible) database with Easy create, then deleted the instance and cluster.

* Acquired the ability to connect between the web interface and CLI to manage AWS resources in parallel.
* ...
