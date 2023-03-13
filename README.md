# examprep-saac02

This repository offers a collection of exam preparation services and notes for students taking the AWS Certified Solutions Architect - Associate (SAA-C03).

This repository is updated to the Exam SAA-C03 on March 5, 2023.

<!-- TOC -->

- [examprep-saac02](#examprep-saac02)
- [Introduction](#introduction)
- [Things to learn and research](#things-to-learn-and-research)
- [Plan](#plan)
- [Complete AWS Certified Solutions Architect - Associate SAA-C03](#complete-aws-certified-solutions-architect---associate-saa-c03)
- [Exam](#exam)
  - [Audience profile](#audience-profile)
  - [Skills measured](#skills-measured)
  - [Response types](#response-types)
  - [Exam results](#exam-results)
  - [Exam content](#exam-content)
- [Project structure](#project-structure)
- [References](#references)

<!-- /TOC -->
---
# Introduction

---
# Things to learn and research
In no particular order, my plan is to use the following resources to learn and research.

| Title | Type | Author |
|-----|-----|-----|
| AWS Certified Solutions Architect - Associate (SAA-C03) | E-Course | [A Cloud Guru](https://learn.acloud.guru/course/certified-solutions-architect-associate/overview) |

---
# Plan
- [ ] Complete AWS Certified Solutions Architect - Associate (SAA-C03) | A Cloud Guru

---
# Complete AWS Certified Solutions Architect - Associate (SAA-C03)

<details>
<summary>Chapter 4. Identity and Access Management IAM</summary>

- [Chapter 4. Identity and Access Management IAM](doc/acloudguru/chapter4.md#chapter-4-identity-and-access-management-iam)
  - [Securing the Root Account](doc/acloudguru/chapter4.md#securing-the-root-account)
  - [Assign Permissions Using IAM Policy Documents Consisting of JSON](doc/acloudguru/chapter4.md#assign-permissions-using-iam-policy-documents-consisting-of-json)
  - [IAM Credentials](doc/acloudguru/chapter4.md#iam-credentials)
  - [Lab 4. Create and Assume Roles in AWS](doc/acloudguru/chapter4.md#lab-4-create-and-assume-roles-in-aws)
    - [Create the Correct S3 Restricted Policies and Roles](doc/acloudguru/chapter4.md#create-the-correct-s3-restricted-policies-and-roles)
</details>
<details>
<summary>Chapter 5. Simple Storage Service S3</summary>

- [Chapter 5. Simple Storage Service S3](doc/acloudguru/chapter5.md#chapter-5-simple-storage-service-s3)
  - [S3 Overview](doc/acloudguru/chapter5.md#s3-overview)
    - [Key-Value Store](doc/acloudguru/chapter5.md#key-value-store)
    - [S3 Standard](doc/acloudguru/chapter5.md#s3-standard)
    - [Tiered Storage](doc/acloudguru/chapter5.md#tiered-storage)
    - [Securing Your Data](doc/acloudguru/chapter5.md#securing-your-data)
  - [Securing Your Bucket with S3 Block Public Access](doc/acloudguru/chapter5.md#securing-your-bucket-with-s3-block-public-access)
    - [Object ACLs vs Bucket Policies](doc/acloudguru/chapter5.md#object-acls-vs-bucket-policies)
  - [Hosting a Static Website Using S3](doc/acloudguru/chapter5.md#hosting-a-static-website-using-s3)
  - [Versioning Objects in S3](doc/acloudguru/chapter5.md#versioning-objects-in-s3)
  - [S3 Storage Classes](doc/acloudguru/chapter5.md#s3-storage-classes)
    - [S3 Standard](doc/acloudguru/chapter5.md#s3-standard)
    - [S3 Standard-Infrequent Access S3 Standard-IA](doc/acloudguru/chapter5.md#s3-standard-infrequent-access-s3-standard-ia)
    - [S3 One Zone-Infrequent Access](doc/acloudguru/chapter5.md#s3-one-zone-infrequent-access)
    - [S3 Intelligent Tiering](doc/acloudguru/chapter5.md#s3-intelligent-tiering)
    - [S3 Glacier](doc/acloudguru/chapter5.md#s3-glacier)
    - [Glacier Instant Retrieval](doc/acloudguru/chapter5.md#glacier-instant-retrieval)
    - [Glacier Flexible Retrieval](doc/acloudguru/chapter5.md#glacier-flexible-retrieval)
    - [Glacier Deep Archive](doc/acloudguru/chapter5.md#glacier-deep-archive)
  - [Lifecycle Management with S3](doc/acloudguru/chapter5.md#lifecycle-management-with-s3)
  - [S3 Object Lock and Glacier Vault Lock](doc/acloudguru/chapter5.md#s3-object-lock-and-glacier-vault-lock)
    - [S3 Object Lock Modes](doc/acloudguru/chapter5.md#s3-object-lock-modes)
    - [Retention Periods](doc/acloudguru/chapter5.md#retention-periods)
    - [Legal Holds](doc/acloudguru/chapter5.md#legal-holds)
    - [Glacier Vault Lock](doc/acloudguru/chapter5.md#glacier-vault-lock)
  - [Encrypting S3 Objects](doc/acloudguru/chapter5.md#encrypting-s3-objects)
    - [Types of Encryption](doc/acloudguru/chapter5.md#types-of-encryption)
    - [Enforcing Server-Side Encryption](doc/acloudguru/chapter5.md#enforcing-server-side-encryption)
  - [Optimizing S3 Performance](doc/acloudguru/chapter5.md#optimizing-s3-performance)
    - [S3 Prefixes Explained](doc/acloudguru/chapter5.md#s3-prefixes-explained)
    - [S3 Limitations when using SSE-KMS](doc/acloudguru/chapter5.md#s3-limitations-when-using-sse-kms)
    - [S3 Multipart Uploads](doc/acloudguru/chapter5.md#s3-multipart-uploads)
    - [S3 Byte-Range Downloads](doc/acloudguru/chapter5.md#s3-byte-range-downloads)
  - [Backing up Data with S3 Replication](doc/acloudguru/chapter5.md#backing-up-data-with-s3-replication)
  - [Lab Exercises](doc/acloudguru/chapter5.md#lab-exercises)
    - [Lab 5. Create a Static Website Using Amazon S3](doc/acloudguru/chapter5.md#lab-5-create-a-static-website-using-amazon-s3)
</details>
<details>
<summary>Chapter 6. Elastic Compute Cloud EC2</summary>

- [Chapter 6. Elastic Compute Cloud EC2](doc/acloudguru/chapter6.md#chapter-6-elastic-compute-cloud-ec2)
  - [EC2 Overview](doc/acloudguru/chapter6.md#ec2-overview)
    - [EC2 Pricing Options](doc/acloudguru/chapter6.md#ec2-pricing-options)
  - [Using Roles](doc/acloudguru/chapter6.md#using-roles)
  - [Security Groups and Bootstrap Scripts](doc/acloudguru/chapter6.md#security-groups-and-bootstrap-scripts)
    - [Exam Tips](doc/acloudguru/chapter6.md#exam-tips)
  - [EC2 Metadata and User Data](doc/acloudguru/chapter6.md#ec2-metadata-and-user-data)
  - [Networking with EC2](doc/acloudguru/chapter6.md#networking-with-ec2)
    - [Elastic Network Interface ENI](doc/acloudguru/chapter6.md#elastic-network-interface-eni)
    - [Enhanced Networking EN](doc/acloudguru/chapter6.md#enhanced-networking-en)
    - [Elastic Fabric Adapter EFA](doc/acloudguru/chapter6.md#elastic-fabric-adapter-efa)
    - [Exam Tips](doc/acloudguru/chapter6.md#exam-tips)
  - [Optimizing with EC2 Placement Groups](doc/acloudguru/chapter6.md#optimizing-with-ec2-placement-groups)
    - [Exam Tips](doc/acloudguru/chapter6.md#exam-tips)
  - [Solving Licensing Issues with Dedicated Hosts](doc/acloudguru/chapter6.md#solving-licensing-issues-with-dedicated-hosts)
    - [Exam Tips](doc/acloudguru/chapter6.md#exam-tips)
  - [Timing Workloads with Spot Instances and Spot Fleets](doc/acloudguru/chapter6.md#timing-workloads-with-spot-instances-and-spot-fleets)
    - [Spot Prices](doc/acloudguru/chapter6.md#spot-prices)
    - [Spot Blocks](doc/acloudguru/chapter6.md#spot-blocks)
    - [How to Terminate Spot Instances](doc/acloudguru/chapter6.md#how-to-terminate-spot-instances)
    - [Spot Fleets](doc/acloudguru/chapter6.md#spot-fleets)
    - [Launch Pools](doc/acloudguru/chapter6.md#launch-pools)
    - [Strategies](doc/acloudguru/chapter6.md#strategies)
    - [Exam Tips](doc/acloudguru/chapter6.md#exam-tips)
  - [Lab 6.1. EC2 Instance Bootstrapping](doc/acloudguru/chapter6.md#lab-61-ec2-instance-bootstrapping)
  - [Lab 6.2. Using EC2 Roles and Instance Profiles in AWS](doc/acloudguru/chapter6.md#lab-62-using-ec2-roles-and-instance-profiles-in-aws)
</details>

<details>
<summary>Chapter 11. Monitoring</summary>

- [Chapter 11. Monitoring](doc/acloudguru/chapter11.md#chapter-11-monitoring)
  - [CloudWatch Overview](doc/acloudguru/chapter11.md#cloudwatch-overview)
    - [What Is CloudWatch?](doc/acloudguru/chapter11.md#what-is-cloudwatch)
    - [CloudWatch Features](doc/acloudguru/chapter11.md#cloudwatch-features)
    - [Types of Metrics](doc/acloudguru/chapter11.md#types-of-metrics)
    - [Exam Tips](doc/acloudguru/chapter11.md#exam-tips)
  - [Application Monitoring with CloudWatch Logs](doc/acloudguru/chapter11.md#application-monitoring-with-cloudwatch-logs)
    - [What is CloudWatch Logs?](doc/acloudguru/chapter11.md#what-is-cloudwatch-logs)
    - [CloudWatch Logs Terms](doc/acloudguru/chapter11.md#cloudwatch-logs-terms)
    - [Features](doc/acloudguru/chapter11.md#features)
    - [Exam Tips](doc/acloudguru/chapter11.md#exam-tips)
  - [Monitoring with Amazon Managed Services for Prometheus and Amazon Managed Grafana](doc/acloudguru/chapter11.md#monitoring-with-amazon-managed-services-for-prometheus-and-amazon-managed-grafana)
    - [What is Amazon Managed Grafana?](doc/acloudguru/chapter11.md#what-is-amazon-managed-grafana)
    - [Amazon Managed Grafana Overview](doc/acloudguru/chapter11.md#amazon-managed-grafana-overview)
    - [Use Cases](doc/acloudguru/chapter11.md#use-cases)
    - [What is Amazon Managed Prometheus?](doc/acloudguru/chapter11.md#what-is-amazon-managed-prometheus)
    - [Amazon Managed Prometheus Overview](doc/acloudguru/chapter11.md#amazon-managed-prometheus-overview)
  - [Monitoring Exam Tips](doc/acloudguru/chapter11.md#monitoring-exam-tips)
    - [Questions to Ask in the Exam](doc/acloudguru/chapter11.md#questions-to-ask-in-the-exam)
    - [What to Know for the Exam](doc/acloudguru/chapter11.md#what-to-know-for-the-exam)
  - [Lab 11.1. CloudWatch Logs Monitoring for a Web Server](doc/acloudguru/chapter11.md#lab-111-cloudwatch-logs-monitoring-for-a-web-server)
  - [Lab 11.2. AWS VPC Flow Logs for Network Monitoring](doc/acloudguru/chapter11.md#lab-112-aws-vpc-flow-logs-for-network-monitoring)
</details>
<details>
<summary>Chapter 16. Security</summary>

- [Chapter 16. Security](doc/acloudguru/chapter16.md#chapter-16-security)
  - [DDoS Overview](doc/acloudguru/chapter16.md#ddos-overview)
    - [Layer 4 Attacks](doc/acloudguru/chapter16.md#layer-4-attacks)
    - [Amplication Attacks](doc/acloudguru/chapter16.md#amplication-attacks)
    - [Layer 7 Attacks](doc/acloudguru/chapter16.md#layer-7-attacks)
  - [Logging API Calls with CloudTrail](doc/acloudguru/chapter16.md#logging-api-calls-with-cloudtrail)
    - [What is logged?](doc/acloudguru/chapter16.md#what-is-logged)
    - [Exam Tips](doc/acloudguru/chapter16.md#exam-tips)
  - [Protecting Applications with Shield](doc/acloudguru/chapter16.md#protecting-applications-with-shield)
    - [Exam Tips](doc/acloudguru/chapter16.md#exam-tips)
  - [Filtering Traffic with AWS WAF](doc/acloudguru/chapter16.md#filtering-traffic-with-aws-waf)
    - [Behaviours and Conditions](doc/acloudguru/chapter16.md#behaviours-and-conditions)
    - [Exam Tips](doc/acloudguru/chapter16.md#exam-tips)
  - [Guarding Your Network with GuardDuty](doc/acloudguru/chapter16.md#guarding-your-network-with-guardduty)
    - [GuardDuty Features](doc/acloudguru/chapter16.md#guardduty-features)
    - [Threat Detection with AI](doc/acloudguru/chapter16.md#threat-detection-with-ai)
    - [GuardDuty Costs](doc/acloudguru/chapter16.md#guardduty-costs)
  - [Centralizing WAF Management via AWS Firewall Manager](doc/acloudguru/chapter16.md#centralizing-waf-management-via-aws-firewall-manager)
    - [Manage Security across Multiple Accounts](doc/acloudguru/chapter16.md#manage-security-across-multiple-accounts)
    - [Benefits](doc/acloudguru/chapter16.md#benefits)
    - [Exam Tips](doc/acloudguru/chapter16.md#exam-tips)
  - [Monitoring S3 Buckets with Macie](doc/acloudguru/chapter16.md#monitoring-s3-buckets-with-macie)
    - [Personally Identifiable Information PII](doc/acloudguru/chapter16.md#personally-identifiable-information-pii)
    - [Automated Analysis of Data](doc/acloudguru/chapter16.md#automated-analysis-of-data)
    - [Macie Alerts](doc/acloudguru/chapter16.md#macie-alerts)
    - [Exam Tips](doc/acloudguru/chapter16.md#exam-tips)
  - [Securing Operating Systems with Inspector](doc/acloudguru/chapter16.md#securing-operating-systems-with-inspector)
    - [Assessment Findings and Types](doc/acloudguru/chapter16.md#assessment-findings-and-types)
    - [How Does It Work?](doc/acloudguru/chapter16.md#how-does-it-work)
  - [Managing Encryption Keys with KMS and CloudHSM](doc/acloudguru/chapter16.md#managing-encryption-keys-with-kms-and-cloudhsm)
    - [Generating a CMK](doc/acloudguru/chapter16.md#generating-a-cmk)
    - [Key Rotation](doc/acloudguru/chapter16.md#key-rotation)
    - [Key Policies](doc/acloudguru/chapter16.md#key-policies)
    - [CloudHSM](doc/acloudguru/chapter16.md#cloudhsm)
  - [Storing Your Secrets in Secrets Manager](doc/acloudguru/chapter16.md#storing-your-secrets-in-secrets-manager)
    - [What can be stored?](doc/acloudguru/chapter16.md#what-can-be-stored)
    - [Automatic Rotation](doc/acloudguru/chapter16.md#automatic-rotation)
    - [Exam Tips](doc/acloudguru/chapter16.md#exam-tips)
  - [Storing Your Secrets in Parameter Store](doc/acloudguru/chapter16.md#storing-your-secrets-in-parameter-store)
    - [Costs & Limitations](doc/acloudguru/chapter16.md#costs--limitations)
    - [Exam Tips](doc/acloudguru/chapter16.md#exam-tips)
  - [Temporarily Sharing S3 Objects Using Presigned URLs or Cookies](doc/acloudguru/chapter16.md#temporarily-sharing-s3-objects-using-presigned-urls-or-cookies)
    - [Presigned URLS and Access](doc/acloudguru/chapter16.md#presigned-urls-and-access)
    - [Presigned Cookies](doc/acloudguru/chapter16.md#presigned-cookies)
  - [Advanced IAM Policy Documents](doc/acloudguru/chapter16.md#advanced-iam-policy-documents)
    - [Amazon Resource Names ARNs](doc/acloudguru/chapter16.md#amazon-resource-names-arns)
    - [IAM Policies](doc/acloudguru/chapter16.md#iam-policies)
    - [Permission Boundaries](doc/acloudguru/chapter16.md#permission-boundaries)
    - [Exam Tips](doc/acloudguru/chapter16.md#exam-tips)
  - [AWS Certificate Manager ACM](doc/acloudguru/chapter16.md#aws-certificate-manager-acm)
  - [Auditing Continuously with AWS Audit Manager](doc/acloudguru/chapter16.md#auditing-continuously-with-aws-audit-manager)
    - [Exam Tips](doc/acloudguru/chapter16.md#exam-tips)
  - [Downloading Compliance Documents from AWS Artifact](doc/acloudguru/chapter16.md#downloading-compliance-documents-from-aws-artifact)
    - [Exam Tips](doc/acloudguru/chapter16.md#exam-tips)
  - [Authenticating Access with AWS Cognito](doc/acloudguru/chapter16.md#authenticating-access-with-aws-cognito)
    - [Features](doc/acloudguru/chapter16.md#features)
    - [Use Cases](doc/acloudguru/chapter16.md#use-cases)
    - [User Pools & Identity Pools](doc/acloudguru/chapter16.md#user-pools--identity-pools)
    - [How it Works Broadly](doc/acloudguru/chapter16.md#how-it-works-broadly)
    - [Cognito Sequence](doc/acloudguru/chapter16.md#cognito-sequence)
  - [Analyzing Root Cause Using Amazon Detective](doc/acloudguru/chapter16.md#analyzing-root-cause-using-amazon-detective)
    - [Sources and Use Cases](doc/acloudguru/chapter16.md#sources-and-use-cases)
    - [Exam Tips](doc/acloudguru/chapter16.md#exam-tips)
  - [Protecting VPCs using AWS Network Firewall](doc/acloudguru/chapter16.md#protecting-vpcs-using-aws-network-firewall)
    - [Benefits and Use Cases](doc/acloudguru/chapter16.md#benefits-and-use-cases)
    - [Exam Tips](doc/acloudguru/chapter16.md#exam-tips)
  - [Leveraging AWS Security Hub for Collecting Security Data](doc/acloudguru/chapter16.md#leveraging-aws-security-hub-for-collecting-security-data)
  - [Lab 11.1. Using Secrets Manager to Authenticate with an RDS Database Using Lambda](doc/acloudguru/chapter16.md#lab-111-using-secrets-manager-to-authenticate-with-an-rds-database-using-lambda)
    - [Introduction](doc/acloudguru/chapter16.md#introduction)
</details>
<details>
<summary>Chapter 17. Automation</summary>

- [Chapter 17. Automation](doc/acloudguru/chapter17.md#chapter-17-automation)
  - [Why Do We Automate](doc/acloudguru/chapter17.md#why-do-we-automate)
    - [Benefits of Automation](doc/acloudguru/chapter17.md#benefits-of-automation)
    - [Exam Tips](doc/acloudguru/chapter17.md#exam-tips)
  - [CloudFormation](doc/acloudguru/chapter17.md#cloudformation)
    - [Exam Tips](doc/acloudguru/chapter17.md#exam-tips)
  - [Elastic Beanstalk](doc/acloudguru/chapter17.md#elastic-beanstalk)
    - [Exam Tips](doc/acloudguru/chapter17.md#exam-tips)
  - [Systems Manager](doc/acloudguru/chapter17.md#systems-manager)
    - [Feature Overview](doc/acloudguru/chapter17.md#feature-overview)
    - [Exam Tips](doc/acloudguru/chapter17.md#exam-tips)
  - [Lab 17.1. Getting Started with CloudFormation](doc/acloudguru/chapter17.md#lab-171-getting-started-with-cloudformation)
    - [Introduction](doc/acloudguru/chapter17.md#introduction)
    - [Create a CloudFormation Stack](doc/acloudguru/chapter17.md#create-a-cloudformation-stack)
    - [Delete CloudFormation Stack](doc/acloudguru/chapter17.md#delete-cloudformation-stack)
</details>

---
# Exam

## Audience profile

## Skills measured

## Response types

## Exam results

## Exam content

---
# Project structure

```
root/                             <-- Root of your Project
|- .gitignore                     <-- GitHub ignore
|- README.md                      <-- GitHub README markdown
   +- doc/                        <-- Holds any doc files
   +- img/                        <-- Holds any image files
   +- src/                        <-- Holds any source code
```

---
# References
The following resources were used as a single-use reference.

| Title | Type | Author |
|-----|-----|-----|
| A Cloud Guru | Web | [Link](https://acloud.guru) |

