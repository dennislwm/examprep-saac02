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
  - [Exploring with Cost Explorer](#exploring-with-cost-explorer)
  - [Using AWS Budgets](#using-aws-budgets)
  - [Optimizing Costs with AWS Cost and Usage Reports](#optimizing-costs-with-aws-cost-and-usage-reports)
  - [Reducing Compute Spend Using Savings Plans and AWS Compute Optimizer](#reducing-compute-spend-using-savings-plans-and-aws-compute-optimizer)
  - [Auditing with Trusted Advisor](#auditing-with-trusted-advisor)
  - [Enforcing Account Governance via AWS Control Tower](#enforcing-account-governance-via-aws-control-tower)
  - [Managing Software Licenses in AWS with AWS License Manager](#managing-software-licenses-in-aws-with-aws-license-manager)
  - [Monitoring Health Events in the AWS Service Catalog and AWS Proton](#monitoring-health-events-in-the-aws-service-catalog-and-aws-proton)
  - [Optimizing Architectures with the AWS Well-Archtected Tool](#optimizing-architectures-with-the-aws-well-archtected-tool)
  - [Governance Exam Tips](#governance-exam-tips)
  - [Lab 19. Using AWS Tags and Resource Groups](#lab-19-using-aws-tags-and-resource-groups)
    - [Introduction](#introduction)
    - [Runbooks](#runbooks)
      - [Create](#create)

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

---
## Exploring with Cost Explorer

---
## Using AWS Budgets

---
## Optimizing Costs with AWS Cost and Usage Reports

---
## Reducing Compute Spend Using Savings Plans and AWS Compute Optimizer

---
## Auditing with Trusted Advisor

---
## Enforcing Account Governance via AWS Control Tower

---
## Managing Software Licenses in AWS with AWS License Manager

---
## Monitoring Health Events in the AWS Service Catalog and AWS Proton

---
## Optimizing Architectures with the AWS Well-Archtected Tool

---
## Governance Exam Tips

---
## Lab 19. Using AWS Tags and Resource Groups

### Introduction

You will use SQS to trigger a Lambda function, which will process messages from the SQS queue and insert the message data as records into a DynamoDB table.

### Runbooks

1. Create

<details>
<summary>Click here to start Lab 19.</summary>

#### 1. Create

</details>