# Chapter 19. Governance

<!-- TOC -->

- [Chapter 19. Governance](#chapter-19-governance)
  - [Managing Accounts with Organizations](#managing-accounts-with-organizations)
    - [AWS Organisations Key Features](#aws-organisations-key-features)
    - [Exam Tips](#exam-tips)
  - [Sharing Resources with AWS Resource Access Manager RAM](#sharing-resources-with-aws-resource-access-manager-ram)
    - [Benefits of Sharing with AWS RAM](#benefits-of-sharing-with-aws-ram)
    - [Exam Tips](#exam-tips)
  - [Setting Up Cross-Account Role Access](#setting-up-cross-account-role-access)
    - [Steps to Setup Cross-Account Role Access](#steps-to-setup-cross-account-role-access)
    - [Exam Tips](#exam-tips)
  - [Inventory Management with AWS Config](#inventory-management-with-aws-config)
    - [State of Your Architecture](#state-of-your-architecture)
    - [Exam Tips](#exam-tips)
  - [Offloading Active Directory to Directory Service](#offloading-active-directory-to-directory-service)
    - [AWS Directory Service Types](#aws-directory-service-types)
  - [Exploring with Cost Explorer](#exploring-with-cost-explorer)
    - [Cost Explorer Features](#cost-explorer-features)
  - [Using AWS Budgets](#using-aws-budgets)
    - [Types of Budgets](#types-of-budgets)
  - [Optimizing Costs with AWS Cost and Usage Reports](#optimizing-costs-with-aws-cost-and-usage-reports)
    - [AWS CURS Use Cases](#aws-curs-use-cases)
    - [Exam Tips](#exam-tips)
  - [AWS Compute Optimizer](#aws-compute-optimizer)
    - [AWS Compute Optimizer Resources](#aws-compute-optimizer-resources)
    - [Supported Account Types](#supported-account-types)
    - [Exam Tips](#exam-tips)
  - [Reducing Compute Spend Using AWS Savings Plans](#reducing-compute-spend-using-aws-savings-plans)
    - [What Are Saving Plans?](#what-are-saving-plans)
    - [Saving Plan Types](#saving-plan-types)
    - [Using and Applying Saving Plans](#using-and-applying-saving-plans)
  - [Auditing with Trusted Advisor](#auditing-with-trusted-advisor)
    - [Supported Checks](#supported-checks)
    - [Exam Tips](#exam-tips)
  - [Enforcing Account Governance via AWS Control Tower](#enforcing-account-governance-via-aws-control-tower)
    - [Features and Terms](#features-and-terms)
    - [Guardrails](#guardrails)
  - [Managing Software Licenses in AWS with AWS License Manager](#managing-software-licenses-in-aws-with-aws-license-manager)
  - [Monitoring Health Events in the AWS Health Dashboard](#monitoring-health-events-in-the-aws-health-dashboard)
    - [AWS Health Concepts](#aws-health-concepts)
  - [Standardizing Self-Service Deployments Using AWS Service Catalog and AWS Proton](#standardizing-self-service-deployments-using-aws-service-catalog-and-aws-proton)
    - [Benefits of AWS Service Catalog](#benefits-of-aws-service-catalog)
    - [AWS Proton](#aws-proton)
  - [Optimizing Architectures with the AWS Well-Architected Tool](#optimizing-architectures-with-the-aws-well-architected-tool)
    - [AWS Well-Architected Framework Review](#aws-well-architected-framework-review)
    - [AWS Well-Architected Tool](#aws-well-architected-tool)
  - [Lab 19. Using AWS Tags and Resource Groups](#lab-19-using-aws-tags-and-resource-groups)
    - [Introduction](#introduction)
    - [Runbooks](#runbooks)
      - [Set Up AWS Config](#set-up-aws-config)
      - [Tag an AMI and EC2 Instance](#tag-an-ami-and-ec2-instance)
      - [Tag Applications with the Tag Editor](#tag-applications-with-the-tag-editor)
      - [Create Resource Groups and Use AWS Config Rules for Compliance](#create-resource-groups-and-use-aws-config-rules-for-compliance)

<!-- /TOC -->

---
## Managing Accounts with Organizations

AWS Organizations is a free governance tool that allows you to **create and manage multiple AWS accounts**. With it, you can control your accounts from a single location rather than jumping from account to account.

### AWS Organisations Key Features

* Dedicated Logging Account - **create a specific account dedicated to logging** as a best practice.

* Programmatic Creation - easily **create and destroy new AWS accounts programmatically**.

* Shared Reserved Instances - allow you to **share reserved instances** across all accounts.

* Consolidated Billing - primary account pays the bills for all accounts.

* Service Control Policies - **use an SCP to limit users' permissions** across all accounts and this SCP has precedence over individual account IAM policies.

  * SCPs are the only way to **apply IAM policies to the root account** across all accounts.

  * SCPs can be used to whitelist AWS resources.

  * SCPs can be **used to deny actions** across all accounts.

### Exam Tips

* Use AWS Organizations to **ensure logs are centralized and restrict anyone from making changes to them using SCPs**.

---
## Sharing Resources with AWS Resource Access Manager (RAM)

AWS RAM is a free service that allows you to **share AWS resources with other accounts within your Organization** to prevent deduplication.

### Benefits of Sharing with AWS RAM

* **Share multiple AWS resources across all accounts**, including transit gateways, license manager, VPC subnets, Route53 resolver,dedicated hosts, etc.

* **Associate a permission with each resource type**.

* **Choose principals that are allowed to access**, e.g. specific AWS accounts or all accounts within your Organizations.

### Exam Tips

* VPC subnets that are shared, but not the default VPC, will appear in a new VPC (shared) in the AWS Console of the destination account.

* Instances that are created within a shared VPC subnet will be accessible by all accounts, however, they do not appear in the AWS Console of all accounts, but only the source account that created these instances.

* **Use AWS RAM for sharing resources within the same region**, otherwise **use VPC peering for sharing across regions** or across accounts not within an Organization.

---
## Setting Up Cross-Account Role Access

As the number of AWS accounts increases, you'll need to set up cross-account role access to **set up temporary access you can easily control**.

### Steps to Setup Cross-Account Role Access

1. Create an IAM role that has least privilege access to resources.

2. Grant access to who can assume the IAM role.

3. User will be able to the new assume role.

### Exam Tips

* Anytime credentials or security are mentioned, scan the answers looking for cross-account role access.

* It is preferred to create cross-account role access, rather than IAM users, everywhere.

---
## Inventory Management with AWS Config

AWS Config is an inventory management and control tool that allows you to **show the history of your infrastructure** along with **creating rules to make sure it conforms to the best practices** you've laid out.

### State of Your Architecture

* Query - Easily **discover what architecture you have** in your account. You can query be resource type, tag, and even see deleted resources.

* Enforce - **Create rules to flag when something is violated**. You can be alerted or even have it automatically fixed using remediation action, e.g. SSM or Lambda.

* Drift - **Discover when your architecture changed in your account**. When or who made that change?

* Consolidation - **Roll up all your results** across all accounts within the same region.

### Exam Tips

* If any question lays out type of standard that needs to be managed across all accounts, think AWS Config.

---
## Offloading Active Directory to Directory Service

AWS Directory Service is a fully managed version of Active Directory (AD).

### AWS Directory Service Types

* Managed Microsoft AD - Entire AD suite in AWS.

* AD Connector - **Creates a tunnel between AWS and your on-premises AD**.

* Simple AD - Standalone AD powered by Linux Samba Active Directory.

---
## Exploring with Cost Explorer

AWS Cost Explorer is an easy-to-use tool that allows you to **visualize your cloud costs** and **generate reports** based on a variety of factors, including resource tags.

### Cost Explorer Features

* Service - Easily break down costs on a service-by-service basis.

* Time - Break down costs by time period.

* Filter - Filter on regions, tags, categories, etc.

* Predictive - Estimate your spend for the upcoming months.

---
## Using AWS Budgets

AWS Budgets allows organizations to easily **plan and set expectations around cloud costs**. You can easily track your ongoing spend and **create alert thresholds to let users know when they're close to exceeding their alloted spend**, and **choose a remediation action to mitigate the budget exceeding**.

### Types of Budgets

You get 2 free budgets every month.

* Cost Budget - How much are we spending? Set a fixed cost budget per period.

* Usage Budget - How much are we using? Use tags to create very specific budgets.

* Reservation Budget - Are we being efficient with our Reserved instances?

* Saving Plans Budget - Is what we're doing covered by our savings plan?

---
## Optimizing Costs with AWS Cost and Usage Reports

AWS Cost and Usage Reports (CUR) is the most comprehensive set of cost and usage data available for AWS spending.

* **Publish billing reports to AWS S3** for centralize collection.

* Break costs down by the time span, service and resource, or by tags.

* AWS CUR updates reports in S3 buckets once a day using CSV formats.

* Easily integrate with Amazon Athena, Redshift, or QuickSight.

### AWS CURS Use Cases

* **Use within AWS Organizations** for entire OU groups or individual member accounts.

* **Track Saving Plans utilizations**, charges, and current allocations.

* **Monitor on-demand capacity reservations**.

* Break down your AWS data transfer charges.

* Dive deeper into cost allocation tag resource spending.

### Exam Tips

* AWS CUR is the most comprehensive and detailed view of your AWS spending.

* Any mention of detailed cost break downs, delivery of daily usage reports, or tracking saving plans utilizations.

---
## AWS Compute Optimizer

AWS Compute Optimizer:

* Optimizes - **Analyzes configurations and utilization metrics** of your AWS resources.

* Reporting - **Reports current usage optimizations** and potential recommendations.

* Graphs - **Provides graphical history data** and projected utilization metrics.

* Informed Decisions - Use graphs, metric data, and **recommendations for moving or resizing resources**.

### AWS Compute Optimizer Resources

* EC2

* ASG

* EBS

* Lambda

### Supported Account Types

* Individual AWS standalone account.

* Individual AWS member account within an AWS organization.

* AWS Organizations management account for all member accounts.

### Exam Tips

* You must **opt in to leverage AWS Compute Optimizer**. After opting in, you should **enable enhanced recommendations** via activation of recommendation preferences (some are paid features).

---
## Reducing Compute Spend Using AWS Savings Plans

### What Are Saving Plans?

* Flexible Pricing - Up to 72% savings on compute.

* Lower Prices - Regardless of instance family, size, OS, tenancy, or regions.

* Variety - AWS Lambda, Fargate, and SageMaker usage.

* Commitments - One-year or three-year pricing commitments.

* Pricing Plan - All Upfront, Partial Upfront, or No Upfront options.

### Saving Plan Types

Three different Saving Plan types:

* Compute Saving Plan - Applies to any EC2, Lambda or Fargate usage (up to 66% savings).

* EC2 Instance Saving Plan - Applies to only EC2 instances of a specific instance family in specific region (up to 72% savings).

* SageMaker Saving Plan - Applies to SageMaker instances regardless of instance family, size, region etc (up to 64% savings).

### Using and Applying Saving Plans

* **View Saving Plans recommendations within your AWS Billing console**.

* Recommendations are automatically calculated to make purchasing easier.

* Add to cart and purchase directly within your AWS account.

* Apply to usage rates after reserved instances are applied and exhausted.

* Consolidated billing family - Applied to account owner first, then spread to other accounts if you enable sharing.

---
## Auditing with Trusted Advisor

AWS Trusted Advisor is a freemium fully managed auditing tool that **scans five different parts of your account** to look for places where you improve your adoption of the recommended best practices.

### Supported Checks

* Cost Optimization - Are you spending money on resources that aren't needed?

* Performance - Are your services configured properly?

* Security - Is your AWS architecture full of vulnerabilities?

* Fault Tolerance - Are you protected when something fails?

* Service Limits - Do you have room to scale up?

### Exam Tips

* Alerts - Use alerts to give a heads up to your team.

* Cost - You will need a Business or Enterprise Support plan for the most useful checks.

* Automate - Use EventBridge to kick off Lambda to remediate the problem for you.

---
## Enforcing Account Governance via AWS Control Tower

AWS Control Tower offers the quickest way to create and manage a secure, compliant, multi-account environment based on best practices.

* Orchestration - **Automates account creation** and security controls via other AWS services, such as AWS Config, AWS IAM Identity Centre and AWS Organizations.

* Governance - **Set up and govern an AWS multi-account environment**.

* Extension - **Extends AWS Organizations to prevent governance drift**, and leverages different guardrails.

### Features and Terms

* Landing Zone - A container that has a well-architected, multi-account environment based on compliance and best practices.

* Guardrails - High-level rules providing continuous governance that are both preventive and detective.

* Account Factory - Configurable account template for standardizing pre-approved configs of new accounts.

* CloudFormation StackSet - Automated deployments of templates deploying repeated resources for governance.

* Shared Accounts - Three shared accounts used by AWS Control Tower, two of which are created during the Landing Zone creation.
  * Management Account - places both preventive and detective guardrails, set up AWS CloudTrail, and enable governance notifications in every new account.
  * Log Archive Account - every new account is configured so that the AWS Config and CloudTrail logs are sent to the Log Archive Account.
  * Audit Account - every new account is configured to so that all configuration events, aggregate security and drift notifications are sent to the respective SNS topics in the Audit Account.

### Guardrails

Guardrails are high-level rules in plain language providing ongoing governance that are both preventive and detective.

* Preventive - Ensures accounts maintain governance by **disallowing violating actions**.
  * Leverages AWS Service Control Policies.
  * Statuses of Enforced or Not Enabled.
  * Supported in all regions.
* Detective - **Detects and alerts on non-compliant resources**.
  * Leverages AWS Config rules.
  * Statuses of Clear, In Violation, or Not Enabled.
  * Only apply to certain regions that supports AWS Control Tower.

---
## Managing Software Licenses in AWS with AWS License Manager

AWS License Manager is a service that simplifies **managing software licenses with different vendors**, e.g. Microsoft, SAP, etc.

* Centralized - **Manage licenses across hybrid cloud environments**.

* Set Usage Limits - Control and visibility into usage of licenses by **enabling license usage limits**.

* Reduce Overages - **Reduces overages and penalties** via inventory tracking and rule-based controls for consumption.

* Versatile - Supports any software based on vCPU, physical cores, sockets, and number of machines.

---
## Monitoring Health Events in the AWS Health Dashboard

AWS Health provides you visibility of resource performance and availability of AWS services or accounts through operational events.

* View how the health events affect your account.

* Timely and relevant information with events.

* View upcoming maintenance tasks that may affect your account.

* Near-instand delivery of notification and alerts to spped up troublehshooting or prevention.

* Automate remediation actions for operational events using EventBridge.

### AWS Health Concepts

* AWS Health Event - Notifications sent on behalf of AWS services.

* Account-specific Event - Events specific to your account.

* Public Event - Events reported on services that are public.

* AWS Health Dashboard - Dashboard showing account and public events.

* Event Type Code - Include the affected services and the specific type of event.

* Event Type Category - Associated category attached to every event.

* Event Status - Reports if the event is Open, Closed, or Upcoming.

* Affected Entities - Which AWS resources are or may be affected by the event.

---
## Standardizing Self-Service Deployments Using AWS Service Catalog and AWS Proton

AWS Service Catalog allows organizations to **create and manage catalogs of approved IT services** as CloudFormation templates.

* Multi-purpose - List things like AMIs, servers, software, databases, and other preconfigured components.

* Centralized - AWS Organizations can centrally managed IT services and maintain compliance.

* End-User Friendly - End users can be allowed to easily deploy pre-approved catalog items within an organization.

* CloudFormation - Catalog templates are written and listed using CF templates.

### Benefits of AWS Service Catalog

* Standardize - **Restrict launching products** to a specific list of pre-approved solutions.

* Self-Service - End users can **browse products and deploy approved services** on their own.

* Access Control - **Add constraints and grant access to products using AWS IAM**.

* Versioning - **Update products to newer versions** and propagate changes automatically.

### AWS Proton

AWS Proton is a service that creates and manages infrastructure and deployment tooling for users as well as serverless and container-based applications.

1. **Automate infrastructure as code (IaC) provisioning** and deployments.

2. Define **standardized infrastructure for your serverless and container-based apps**.

3. **Use templates to define and manage app stacks** that contain all components.

4. **Automatically provisions resources, configures CI/CD, and deploys the code**.

5. Supports AWS CloudFormation and Terraform IaC providers.

---
## Optimizing Architectures with the AWS Well-Architected Tool

### AWS Well-Architected Framework Review

The SIX (6) Pillars are:

* Operational Excellence

* Reliability

* Security

* Performance Efficiency

* Cost Optimization

* Sustainability

### AWS Well-Architected Tool

* Provides a consistent process for measuring cloud architectures.

* Enables assistance with documenting workloads and architectural decisions.

* Guides for making workloads reliable, secure, efficient, and cost effective.

* Measure workloads against years of AWS best practices.

* Intended for specific audiences, such as CTO, architecture, and operational teams.

---
## Lab 19. Using AWS Tags and Resource Groups

### Introduction

You will use SQS to trigger a Lambda function, which will process messages from the SQS queue and insert the message data as records into a DynamoDB table.

### Runbooks

1. Set Up AWS Config

2. Tag an AMI and EC2 Instance

3. Tag Applications with the Tag Editor

4. Create Resource Groups and Use AWS Config Rules for Compliance

<details>
<summary>Click here to start Lab 19.</summary>

#### 1. Set Up AWS Config

1. Navigate to AWS Console > Config and click **1-click setup**.

2. Leave the settings as their defaults and click **Confirm**.

#### 2. Tag an AMI and EC2 Instance

1. Navigate to AWS Console > EC2 > Instances (running) and select any of the instances listed.

2. Right-click on the name of selected instance and select Image and templates > Create image.

3. For the Image name, enter `Base` and click **Create image**.

4. Navigate to AMIs in the left-hand menu, and select the newly created image (available).

5. Click **Launch instance from AMI**, and enter the following details.
  - Name: `My Test Server`
  - Instance type: `t3.micro`
  - Key pair name: `Proceed without a key pair (Not recommended)`

6. In the Network settings > Firewall (security groups) > Select existing security group.

7. In Common security groups > Select the one with `SecurityGroupWeb` in the name.

8. Leave the rest of the default settings and click **Launch instance**.

#### 3. Tag Applications with the Tag Editor

**Module 1 Tagging**

1. Navigate to AWS Console > Resource Groups & Tag Editor > Click **Tag Editor**.

2. In the Find resources to tag section, set the following values:
  - Regions: `us-east-1`
  - Resource types:
    - `AWS::EC2::Instance`
    - `AWS::S3::Bucket`

3. Click **Search resources**.

4. In the Resource search results, set the following values:
  - In the Filter resources search window: `Mod. 1`
    - Select both instances, and click **Clear filters**.
  - In the Filter resources search window: enter `moduleone`
    - Select the listed S3 bucket, and click **Clear filters**.

5. Click **Manage tags of selected resources**.

6. In the Edit tags of all selected resources section, click **Add tag**, and set the following values:
  - Tag key: `Module`
  - Tag value: `Starship Monitor`

7. Click **Review and apply tag changes** > Apply changes to all selected.

**Module 2 Tagging**

8. Click **Search resources**.

9. In the Resource search results, set the following values:
  - In the Filter resources search window: `Mod. 2`
    - Select both instances, and click **Clear filters**.
  - In the Filter resources search window: enter `moduletwo`
    - Select the listed S3 bucket, and click **Clear filters**.

10. Click **Manage tags of selected resources**.

11. In the Edit tags of all selected resources section, click **Add tag**, and set the following values:
  - Tag key: `Module`
  - Tag value: `Warp Drive`

12. Click **Review and apply tag changes** > Apply changes to all selected.

#### 4. Create Resource Groups and Use AWS Config Rules for Compliance

**Create the `Starship Monitor` Resource Group

1. Navigate to AWS Console > Resource Groups & Tag Editor > Click **Create Resource Group**.

2. For Group type, select **Tag based**.

3. In the Tags field, select the following:
  - Tag key: `Module`
  - Optional tag value: `Starship Monitor`

4. Click **Preview group resources** and ensure all three resources show up.

5. In the Group details section, enter a Group name of `Starship-Monitor` and click **Create group**.

**Create the `Warp Drive` Resource Group

6. In the left-hand menu, Click **Create Resource Group**.

7. For Group type, select **Tag based**.

8. In the Tags field, select the following:
  - Tag key: `Module`
  - Optional tag value: `Warp Drive`

9. Click **Preview group resources** and ensure all three resources show up.

10. In the Group details section, enter a Group name of `Warp-Drive` and click **Create group**.

**Use AWS Config Rules for Compliance**

11. Navigate to AWS Console > EC2 > Instances (running) > Select the `My Test Server` instance.

12. In the Details section, copy its AMI ID.

13. Navigate to AWS Console > AWS Config > click **Rules**.

14. Click **Add rule**.
  - For the rule type, select **Add AWS managed rule**.
  - Search for `approved-amis-by-id` in the search box, and select that rule.

15. Click **Next**.
  - For the Name, enter `my_approved_ami_ids`
  - In the Parameters section, paste the AMI ID you just copied into the **Value** field.

16. Click Next > click **Save**.

17. Navigate back to EC2 > Instances (running) > Select all the instances.

18. Click Instance state > Reboot instance.
  - In the Reboot instances? dialog, click **Reboot**.

19. Navigate back to AWS Config, and you should see non-compliant resources.

20. Click the `approved-amis-by-id` rule.
  - Choose one of the non-compliant resource ID, and remember its last four characters.

21. Navigate back to EC2 > Select the instance (by matching the last four characters)
  - In the Details section, identify its AMI ID.

22. Navigate back to AWS Config, and identify the AMI ID under Value in the Parameters section.

23. You should see it doesn't match the AMI ID you noted in EC2, which means the rule successfully identified non-compliant resources.

</details>