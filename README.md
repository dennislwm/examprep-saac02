# examprep-saac02

This repository offers a collection of exam preparation services and notes for students taking the AWS Certified Solutions Architect - Associate (SAA-C02).

This repository is updated to the Exam SAA-C02 on August 6, 2022.

<!-- TOC -->

- [examprep-saac02](#examprep-saac02)
- [Introduction](#introduction)
- [Things to learn and research](#things-to-learn-and-research)
- [Plan](#plan)
- [Complete AWS Certified Solutions Architect - Associate SAA-C02](#complete-aws-certified-solutions-architect---associate-saa-c02)
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
| AWS Certified Solutions Architect - Associate (SAA-C02) | E-Course | [A Cloud Guru](https://learn.acloud.guru/course/certified-solutions-architect-associate/overview) |

---
# Plan
- [ ] Complete AWS Certified Solutions Architect - Associate (SAA-C02) | A Cloud Guru

---
# Complete AWS Certified Solutions Architect - Associate (SAA-C02)

<details>
<summary>Click here to view TOC of AWS Certified Solutions Architect - Associate (SAA-C02)</summary>

- [Chapter 4. Identity and Access Management IAM](doc/acloudguru/chapter4.md#chapter-4-identity-and-access-management-iam)
  - [Securing the Root Account](doc/acloudguru/chapter4.md#securing-the-root-account)
  - [Assign Permissions Using IAM Policy Documents Consisting of JSON](doc/acloudguru/chapter4.md#assign-permissions-using-iam-policy-documents-consisting-of-json)
  - [IAM Credentials](doc/acloudguru/chapter4.md#iam-credentials)
  - [Lab 4. Create and Assume Roles in AWS](doc/acloudguru/chapter4.md#lab-4-create-and-assume-roles-in-aws)
    - [Create the Correct S3 Restricted Policies and Roles](doc/acloudguru/chapter4.md#create-the-correct-s3-restricted-policies-and-roles)
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
</summary>

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
| Google | Web | [Link](https://google.com) |

