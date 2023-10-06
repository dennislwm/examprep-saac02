# Chapter 7. EC2 Instance Storage

<!-- TOC -->

- [Chapter 7. EC2 Instance Storage](#chapter-7-ec2-instance-storage)
    - [EC2 Instance Store](#ec2-instance-store)
    - [Amazon EFS](#amazon-efs)
        - [EFS Storage Class](#efs-storage-class)
        - [EFS Performance](#efs-performance)
    - [References](#references)

<!-- /TOC -->

---
## EC2 Instance Store

EC2 Instance Store has a higher performance than EBS volumes with better I/O performance. However, Instance Store is an ephemeral storage as they lose data if the instance is stopped.

It is primarily used to buffer cache temporary data for high-performance computation.

---
## Amazon EFS

Amazon EFS offers a range of storage classes for Linux-based AMIs (not Windows):
* EFS Standard
* EFS Standard-Infrequent Access (Standard-IA)
* EFS One Zone
* Efs One Zone-Infrequent Access (EFS One Zone-IA)

### EFS Storage Class

|      Type       |   Standard   | Standard-IA  |    One Zone    |   One Zone-IA    |
|:---------------:|:------------:|:------------:|:--------------:|:----------------:|
|     Region      |    Single    |    Single    |     Single     |      Single      |
|       AZ        | Multi (>=3)  | Multi (>=3)  |     Single     |      Single      |
|     Access      |   Frequent   |  Infrequent  |    Frequent    |    Infrequent    |
| Cost (GB-Month) | High ($0.30) | Low ($0.025) | Medium ($0.16) | Lowest ($0.0133) |

Billing cost is based on:
* Storage size
* Data transfer between storage class
* IA read request
* AWS Backup (for One Zone)

### EFS Performance

* Scalability - 1000s of concurrent NFS clients, 10GB+/s throughput, and grow to PB-scale automatically.
* Performance Mode (set at EFS creation time):
  - General Purpose (default) - latency-sensitive use cases.
  - Max I/O - highly parallel, lower latency (big data, media processing).
* Throughput Mode:
  - Bursting - throughput that grows with storage size.
  - Provisioned - throughput that is fixed.
  - Elastic - automatically scales throughput up or down based on workloads.

---
## References

* [EFS storage classes](https://docs.aws.amazon.com/efs/latest/ug/storage-classes.html)