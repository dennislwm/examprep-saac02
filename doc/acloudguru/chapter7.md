# Chapter 7. Elastic Block Storage (EBS) and Elastic File System (EFS)

<!-- TOC -->

- [Chapter 7. Elastic Block Storage EBS and Elastic File System EFS](#chapter-7-elastic-block-storage-ebs-and-elastic-file-system-efs)
  - [EBS Overview](#ebs-overview)
    - [Solid-State Drives SSD](#solid-state-drives-ssd)
    - [Hard Disk Drive HDD](#hard-disk-drive-hdd)
    - [IOPS vs. Throughput](#iops-vs-throughput)
    - [Exam Tips](#exam-tips)
  - [Volumes and Snapshots](#volumes-and-snapshots)
  - [Protecting EBS Volumes with Encryption](#protecting-ebs-volumes-with-encryption)
  - [EC2 Hibernation](#ec2-hibernation)
  - [EFS Overview](#efs-overview)
  - [FSx Overview](#fsx-overview)
  - [Amazon Machine Images: EBS vs. Instance Store](#amazon-machine-images-ebs-vs-instance-store)
  - [AWS Backup](#aws-backup)
  - [EBS Exam Tips](#ebs-exam-tips)
  - [Lab 7. Reduce Storage Costs with EFS](#lab-7-reduce-storage-costs-with-efs)
    - [Introduction](#introduction)

<!-- /TOC -->

---
## EBS Overview

There are different use cases for each type of Elastic Block Storage (EBS), which are storage volumes or virtual hard disk that you can attach to your EC2.

* Production workloads: Designed for mission-critical workloads

* Highly available: Automatically replicated within a single AZ to protect against hardware failures

* Scalable: Dynamically increase capacity and change the volume type with no downtime or performance impact to your live systems

EBS Volume Types:

### Solid-State Drives (SSD)

* General Purpose SSD (`gp2`): 
  - 3 IOPS per GiB, up to a max of 16,000 IOPS per volume
  - `gp2` volumes smaller than 1 TB can burst up to 3,000 IOPS
  - Good for boot volumes or development and test applications that are not latency sensitive
  - Up to 99.9% durability

* General Purpose SSD (`gp3`):
  - Predictable 3,000 IOPS baseline performance and 125 MiB/s regardless of volume size
  - Ideal for application that require high performance at a low cost, such as MySQL, Cassandra, virtual desktops, and Hadoop analystics
  - Customers looking for higher performance can scale up to 16,000 IOPS and 1,000 MiB/s for an additional fee
  - The top performance of `gp3` is four times faster than max throughput of `gp2` volumes
  - Up to 99.9% durability

> Exam Tip: You are not required to memorize IOPS or throughput metrics, nor be given a scenario where you need to choose between `gp2` or `gp3`. However, always choose `gp3` because it's faster.

* Provisioned IOPS SSD (`io1`):
  - Up to 64,000 IOPS per volume, 50 IOPS per GiB
  - Use if you need more than 16,000 IOPS
  - Designed for I/O-intensive applications, large databases, and latency-sensitive workloads
  - Up to 99.9% durability

* Provisioned IOS SSD (`io2`):
  - Latest generation higher durability and more IOPS than `io1`, but at the same price
  - 500 IOPS per GiB, up to 64,000 IOPS per volume
  - 99.999% durability, instead of up to 99.9%
  - I/O-intensive apps, large databases, and latency-sensitive workloads

### Hard Disk Drive (HDD)

* Throughput Optimized HDD (`st1`):
  - Low-cost HDD volume
  - Baseline throughput of 40 MB/s per TB
  - Ability to burst up to 250 MB/s per TB
  - Maximum throughput of 500 MB/s per volume
  - Frequently accessed, throughput-intensive workloads
  - Big data, data warehouses, ETL, and log processing
  - A cost-effective way to store mountains of data
  - Cannot be a boot volume
  - Up to 99.9% durability

* Cold HDD (`sc1`):
  - Lowest cost HDD option
  - Baseline throughput of 12 MB/s per TB
  - Ability to burst up to 80 MB/s per TB
  - Maximum throughput of 250 MB/s per volume
  - A good choice for colder data requiring fewer scans per day
  - Good for applications that need the lowest cost and performance is not a factor
  - Cannot be a boot volume
  - Up to 99.9% durability

### IOPS vs. Throughput

IOPS:
- Measures the number of read and write operations per second
- Important metric for quick transactions, low-latency apps, transactional workloads
- The ability to action reads and writes very quickly
- Choose Provisioned IOPS SSD (`io1` or `io2`)

Throughput:
- Measures the number of bits read or written per second (MB/s)
- Important metric for large datasets, large I/O sizes, complex queries
- The ability to deal with large datasets
- Choose Throughput Optimized HDD (`st1`)

### Exam Tips

* Boot disk for OS choose `gp3` (we won't be given a choice of choosing `gp2`)

* Low-latency disk for transactional apps choose `io2` (we won't be given a choice of choosing `io1`)

* If you need more than 16,000 IOPS per volume choose `io2` over `gp3`

* If you need the lowest cost EBS choose `sc1`

---
## Volumes and Snapshots

---
## Protecting EBS Volumes with Encryption

---
## EC2 Hibernation

---
## EFS Overview

---
## FSx Overview

---
## Amazon Machine Images: EBS vs. Instance Store

---
## AWS Backup

---
## EBS Exam Tips

---
## Lab 7. Reduce Storage Costs with EFS

<details>
<summary>Click here to start Lab 7.</summary>

### Introduction

</details>