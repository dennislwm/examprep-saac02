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
  - [Hosting a Static Website Using S3](#hosting-a-static-website-using-s3)
  - [Versioning Objects in S3](#versioning-objects-in-s3)
  - [S3 Storage Classes](#s3-storage-classes)
    - [S3 Standard](#s3-standard)
    - [S3 Standard-Infrequent Access S3 Standard-IA](#s3-standard-infrequent-access-s3-standard-ia)
    - [S3 One Zone-Infrequent Access](#s3-one-zone-infrequent-access)
    - [S3 Intelligent Tiering](#s3-intelligent-tiering)
    - [S3 Glacier](#s3-glacier)
    - [Glacier Instant Retrieval](#glacier-instant-retrieval)
    - [Glacier Flexible Retrieval](#glacier-flexible-retrieval)
    - [Glacier Deep Archive](#glacier-deep-archive)
  - [Lifecycle Management with S3](#lifecycle-management-with-s3)
  - [S3 Object Lock and Glacier Vault Lock](#s3-object-lock-and-glacier-vault-lock)
    - [S3 Object Lock Modes](#s3-object-lock-modes)
    - [Retention Periods](#retention-periods)
    - [Legal Holds](#legal-holds)
    - [Glacier Vault Lock](#glacier-vault-lock)
  - [Encrypting S3 Objects](#encrypting-s3-objects)
    - [Types of Encryption](#types-of-encryption)
    - [Enforcing Server-Side Encryption](#enforcing-server-side-encryption)
  - [Optimizing S3 Performance](#optimizing-s3-performance)
    - [S3 Prefixes Explained](#s3-prefixes-explained)
    - [S3 Limitations when using SSE-KMS](#s3-limitations-when-using-sse-kms)
    - [S3 Multipart Uploads](#s3-multipart-uploads)
    - [S3 Byte-Range Downloads](#s3-byte-range-downloads)
  - [Backing up Data with S3 Replication](#backing-up-data-with-s3-replication)
  - [Lab Exercises](#lab-exercises)
    - [Lab 5. Create a Static Website Using Amazon S3](#lab-5-create-a-static-website-using-amazon-s3)
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

