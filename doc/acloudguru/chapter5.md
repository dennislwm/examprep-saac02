# Chapter 5. Simple Storage Service (S3)

<!-- TOC -->

- [Chapter 5. Simple Storage Service S3](#chapter-5-simple-storage-service-s3)
  - [S3 Overview](#s3-overview)
    - [Key-Value Store](#key-value-store)
    - [S3 Standard](#s3-standard)
    - [Tiered Storage](#tiered-storage)
    - [Securing Your Data](#securing-your-data)
  - [Securing Your Bucket with S3 Block Public Access](#securing-your-bucket-with-s3-block-public-access)
    - [Object ACLs vs Bucket Policies](#object-acls-vs-bucket-policies)
  - [Lab Exercises](#lab-exercises)
    - [Lab 5.1.](#lab-51)

<!-- /TOC -->

---
## S3 Overview

* Unlimited storage of objects up to 5 TB in size
  - Key-value store
* Universal namespace (all AWS accounts share the S3 namespace)
  - Example S3 URL: https://BUCKET.s3.REGION.amazonaws.com/
* When you upload a file to an S3 bucket, you will receive an `HTTP 200` code if the upload was successful
* Strong read-after-write consistency
  - Example: After a write of a new object (PUT), you can immediately perform a read request on the latest version of the object

### Key-Value Store

* Key: name of the object
  - Example: `ralph.jpg`
* Value: the data itself
* Version ID: important for storing multiple versions of the same object
* Metadata: Data about the data you are storing
  - Example: `content-type`, `last-modified`, etc

### S3 Standard

* Data is spread across multiple devices and facilities to ensure availablity and durability
  - At least 3 different availability zones (AZs)
  - Built for service availability of 99.95% to 99.99% depending on the S3 tier
  - Designed for 99.999999999% (9 decimal places) durability for data stored in S3

### Tiered Storage

* S3 offers a range of storage classes designed for different use cases
* Lifecycle management: define rules to automatically transition objects to a cheaper storage tier or delete objects
* Versioning: all versions of an object are stored and can be retrieved, including deleted objects

### Securing Your Data

* Server-Side Encryption: set a default encryption on a bucket to encrypt all new objects when they are stored
* Access Control Lists (ACLs): define which AWS accounts or groups are granted access and the type of access
* Bucket Policies: specify what actions are allowed or denied

> Note: Use ACLs for granular access permissions on individual objects

## Securing Your Bucket with S3 Block Public Access

### Object ACLs vs Bucket Policies

* Object ACLs work on an individual object level
* Bucket policies work on an entire bucket level

---
## Lab Exercises

### Lab 5.1.