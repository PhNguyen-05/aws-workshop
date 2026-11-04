---
title: "Week 2 Worklog"
date: 2026-10-12
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---


### Week 2 Objectives:

* Understand the role of cost management when operating systems on AWS and get to know the AWS Budgets service.
* Distinguish the 4 budget types: Cost Budget, Usage Budget, RI Budget, and Savings Plans Budget.
* Practice creating a budget from a template and a customized budget in AWS Billing and Cost Management, and configure email alert thresholds.
* Learn about the AWS Support plans (Basic, Business Support+, Enterprise Support, Unified Operations) and their response times.
* Understand the process of accessing AWS Support Center, creating and managing support requests, and choosing an appropriate severity level.


### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Learn the overview of AWS Budgets: <br>&emsp; + Purpose: set budgets and send alerts when costs exceed (or are forecasted to exceed) the allowance <br>&emsp; + 4 budget types: Cost, Usage, RI, Savings Plans <br>&emsp; + Benefits: cost control, timely alerts, financial planning, resource optimization, cost allocation <br> - Learn the steps to set up budgets effectively: define goals, analyze current costs, set alert thresholds (typically 80% and 100%), configure notifications, monitor and adjust <br> - Distinguish Reserved Instances from Savings Plans | 12/10/2026 | 12/10/2026 | <https://000007.awsfcaj.com/> |
| 3 | - **Practice:** <br>&emsp; + Go to **AWS Billing and Cost Management → Budgets → Create a budget** <br>&emsp; + Create a budget from a template: choose **Use a template (simplified)** → **Monthly cost budget**, set the name, monthly amount, and alert thresholds <br>&emsp; + View budget history and the alert types available in the template <br>&emsp; + Create a customized **Cost Budget**: name it `Monthly`, choose Period, Recurring/Expiring, Fixed/Monthly Budget Planning, enter the Budgeted amount, scope **All AWS services**, **Unblended costs**, add an alert threshold | 13/10/2026 | 13/10/2026 | <https://000007.awsfcaj.com/1-create-budget/> <br> <https://000007.awsfcaj.com/2-cost-budgets/> |
| 4 | - **Practice:** <br>&emsp; + Create a **Usage Budget**: choose **Usage type groups**, **EC2: ELB - Running Hours**, set the running-hours limit, configure the alert threshold and email <br>&emsp; + View **Budget health** and **Budget history** <br>&emsp; + Review the process of creating an **RI Budget** (Reservation budget) and a **Savings Plans Budget** (demonstration only, since they require upfront payment/long-term commitment) <br>&emsp; + **Clean up:** select a budget → **Actions → Delete** → confirm deletion, repeat for all created budgets | 14/10/2026 | 14/10/2026 | <https://000007.awsfcaj.com/3-usage-budget/> <br> <https://000007.awsfcaj.com/4-reservation-budget/> <br> <https://000007.awsfcaj.com/5-saving-plans-budget/> <br> <https://000007.awsfcaj.com/6-clean-up/> |
| 5 | - Learn about **AWS Support** and its support plans: <br>&emsp; + Basic <br>&emsp; + Business Support+ <br>&emsp; + Enterprise Support <br>&emsp; + Unified Operations <br> - Compare the features and response times of each plan <br> - Learn how to access AWS Support: support request types and the process of changing a support plan | 15/10/2026 | 15/10/2026 | <https://000009.awsfcaj.com/> <br> <https://000009.awsfcaj.com/1-support-plans/> <br> <https://000009.awsfcaj.com/2-access-support/> |
| 6 | - Learn how to manage support requests in AWS Support Center: <br>&emsp; + Create a support request <br>&emsp; + Select an appropriate severity level <br>&emsp; + Track and respond to requests <br> - Take note of the security considerations when creating a support case <br> - Summarize week 2 knowledge and write the worklog | 16/10/2026 | 16/10/2026 | <https://000009.awsfcaj.com/3-manage-cases/> |


### Practice and learning details:

#### Lab 7: Cost Management with AWS Budgets

**1. AWS Budgets overview**

* AWS Budgets lets you set budgets and send alerts when costs exceed the allowance (or are forecasted to exceed it).
* Four budget types:
  * **Cost Budget:** sends alerts when total costs exceed the threshold.
  * **Usage Budget:** sends alerts when usage of the selected service exceeds the threshold (for example, EC2 running hours).
  * **RI Budget:** sends alerts based on the usage of prepaid Reserved Instances.
  * **Savings Plans Budget:** sends alerts based on the usage of services covered by Savings Plans.
* Reserved Instances and Savings Plans are both ways to reduce costs by committing to usage for 1 - 3 years. Savings Plans is the newer, more flexible model with equivalent discounts; for EC2, Savings Plans is recommended.
* Benefits: proactive cost control, timely alerts, support for financial planning and cost forecasting, identifying high-cost services to optimize, and easy cost allocation to projects/departments/customers.
* Steps to set up budgets effectively: (1) define budget goals, (2) analyze current costs, (3) set appropriate alert thresholds (typically 80% and 100%), (4) configure notifications to the right authorized people, (5) monitor and adjust regularly.

**2. Create a budget using a template**

* Open the AWS Management Console and find **AWS Billing and Cost Management**.
* Choose **Budgets** from the left menu, then click **Create a budget**.
* Select **Use a template (simplified)**, then under Templates choose **Monthly cost budget**.
* Enter the budget name, monthly budget amount, and alert thresholds, then click **Create budget**.
* Verify the budget was created, and view the budget history and the alert types available in the template.
* Note: budget alerts only notify, they **do not automatically stop resources** when the budget is exceeded; combine with AWS Service Quotas or IAM policies to control usage. Set proper permissions so only authorized personnel can edit or delete budgets.

**3. Create a Cost Budget (Customize)**

* Note: new AWS accounts may initially only be able to create a Cost Budget; other budget types may not be available right away.
* In **Budget setup** choose **Customize**, under **Budget types** choose **Cost budget**, then click **Next**.
* In **Set your budget**:
  * **Budget name:** `Monthly`
  * **Period:** Daily / Monthly / Quarterly / Annually
  * **Budget effective dates:** Recurring Budget (repeats periodically) or Expiring Budget (applies only once). AWS Budgets uses the UTC time zone.
  * **Specify your monthly budget:** Fixed (same amount each period) or Monthly Budget Planning (different amounts per month); enter the **Budgeted amount**.
* **Budget scope:** choose **All AWS services**; **Aggregate costs by:** **Unblended costs**; click **Next**.
* Configure alerts: choose **Add an alert threshold**, set the alert details, click **Next**; review the actions (if any), click **Next**.
* Review all settings, click **Create budget**, and confirm the budget was created successfully.
* Recommendation: set multiple alert thresholds (for example 50%, 80%, 100%) to control costs proactively.

**4. Create a Usage Budget**

* The process is similar to a Cost Budget, with one main difference: a Usage Budget works on **resource usage** rather than cost.
* In **Budget setup** choose **Customize**, under **Budget types** choose **Usage budget**, then click **Next**.
* Enter the **Budget name**; under **Budget against** choose **Usage type groups** and select **EC2: ELB - Running Hours** to monitor EC2 running hours.
* Configure **Set budget amount**: Period (Daily/Monthly/Quarterly/Annually), Budget renewal type (Recurring/Expiring), Budgeting method (Fixed/Planned), and the maximum usage hours.
* Keep the defaults in **Budget scope** and click **Next**.
* In **Configure alerts**: choose **Add an alert threshold**, set the threshold (% of budget), add the email to receive notifications, and click **Next**.
* Review and click **Create budget**.
* After creation, view **Budget health** to compare current usage against the budget and **Budget history** to track trends.
* Especially useful for hourly-billed services like EC2. Only send notifications to the people responsible for cost management to avoid leaking financial information.

**5. Create an RI Budget (demonstration)**

* Because Reserved Instances require upfront payment, this part is for demonstration only.
* In **Budget setup** choose **Customize** → **Reservation budget** → **Next**.
* Set the budget name, configure the **Coverage threshold**, and configure the **Budget scope**.
* In **Alert setting** enter the email, choose **Next**, then **Create budget**.
* Purpose: monitor Reserved Instance utilization to save costs; set appropriate alert thresholds to optimize usage.

**6. Create a Savings Plans Budget (demonstration)**

* The process is very similar to an RI Budget; the difference is that a Reservation Budget relates to Reserved Instances while a Savings Plans Budget relates to Savings Plans commitments.
* In **Budget setup** choose **Customize** → **Savings Plans budget** → **Next**.
* Enter the **Budget name**, configure the **Utilization threshold**, keep the default **Budget scope**, enter the email in **Alert setting**, choose **Next**, then **Create budget**.
* Savings Plans can save up to 72% compared to On-Demand pricing in exchange for committing to a consistent amount of compute usage (USD/hour) for 1 or 3 years. Setting alert thresholds at 80-90% gives time to adjust.

**7. Clean up resources**

* Go to **AWS Billing and Cost Management → Budgets**.
* Select the budget to delete → **Action** → **Delete** → click **Delete** in the confirmation dialog.
* Repeat for all budgets created during practice.
* Deleting a budget **does not affect** running resources, since budgets are only monitoring and alerting tools.

#### Lab 9: Request Support with AWS Support

**1. AWS Support plans**

AWS Support is divided into 4 plans, ordered from low to high: **Basic → Business Support+ → Enterprise Support → Unified Operations**. Higher plans include the services of the lower plans.

| Plan | Key characteristics |
| :--- | :--- |
| **Basic** | - Free for all AWS customers <br> - Support for account and billing issues, service limit increase requests <br> - 24/7 access to: 1-on-1 live chat for account and billing questions, AWS Support Forums, AWS Health Dashboard, documentation and best practice guides |
| **Business Support+** | - Designed for production workloads <br> - 24/7 phone, chat, and email access to Cloud Support Engineers <br> - Unlimited support cases with unlimited contacts <br> - AWS Trusted Advisor with full checks (cost optimization, security, fault tolerance, performance, service limits) <br> - AWS Support API <br> - Third-party software support (common operating systems, platforms, application stack components) <br> - Response times: general guidance < 24 hours; system impaired < 12 hours; production system impaired < 4 hours; production system down < 1 hour |
| **Enterprise Support** | - Includes all Business Support+ features <br> - Technical Account Manager (TAM) <br> - Concierge Support Team for billing and account assistance <br> - Infrastructure Event Management <br> - Well-Architected Reviews <br> - Operations Reviews and Tools <br> - Training and Game Days <br> - Business-critical system down: response < 15 minutes; other severity levels same as Business Support+ |
| **Unified Operations** | - Includes all Enterprise Support features <br> - AWS Managed Services (AMS) <br> - 24/7 proactive monitoring and incident management <br> - Change management <br> - Patch management <br> - Backup and disaster recovery <br> - Continuous security and compliance monitoring <br> - Dedicated service delivery team |

* Choose a support plan that fits the technical requirements and resources of your environment; the plan also affects access to security consulting and compliance services from the AWS team.
* Response times range from 15 minutes to 24 hours depending on the support plan and severity; AWS Support offers multilingual support with global coverage.

**2. Access AWS Support**

* Learn about the support request types and the process of changing a support plan.
* Choosing the right plan helps optimize operating costs while ensuring the necessary level of support for critical workloads.

**3. Manage support requests**

* AWS Support Center is the centralized place to create, manage, and track all support requests.
* Providing detailed and complete information when creating a request helps the AWS team resolve issues faster.
* **Security:** do not share access keys, passwords, or other authentication information in the content of a support request.
* **Choose an appropriate severity:** choosing an inappropriate severity level may lead to unexpected response times or slow down resolution.
* After creating a request, regularly check AWS Support Center to update information and respond promptly to questions from the support team.


### Week 2 Achievements:

* Understood the role of AWS Budgets in cost control and the 4 budget types:
  * Cost Budget
  * Usage Budget
  * RI Budget
  * Savings Plans Budget

* Distinguished Reserved Instances from Savings Plans and understood why Savings Plans is preferred for EC2.

* Learned how to access AWS Billing and Cost Management and create budgets in 2 ways:
  * Using a template (Monthly cost budget)
  * Customized, with full settings: period, recurring/expiring, fixed/planned, scope, aggregate costs

* Practiced creating a Cost Budget and a Usage Budget (tracking EC2 running hours), configuring alert thresholds and notification emails.

* Learned to monitor Budget health and Budget history to evaluate usage against the budget.

* Understood the process of creating an RI Budget and a Savings Plans Budget.

* Learned how to clean up budgets after practice and understood that deleting a budget does not affect running resources.

* Understood that budgets only send alerts and do not automatically block resource usage.

* Learned the 4 AWS Support plans (Basic, Business Support+, Enterprise Support, Unified Operations), with the features and response times of each.

* Understood the process of managing support requests in AWS Support Center, the importance of choosing the right severity, and the security considerations when creating a support case.
* ...
