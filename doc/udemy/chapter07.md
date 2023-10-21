# Chapter 7. EC2 Instance Storage

<!-- TOC -->

- [Chapter 7. EC2 Instance Storage](#chapter-7-ec2-instance-storage)
    - [EC2 Instance Store](#ec2-instance-store)
    - [Amazon EFS](#amazon-efs)
        - [EFS Storage Class](#efs-storage-class)
        - [EFS Performance](#efs-performance)
        - [Latency](#latency)
        - [Throughput](#throughput)
        - [IOPS](#iops)
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

File system performance is typically measured by using the dimensions of latency, throughput, and input/output operations per second (IOPS).

The following configurations impact the performance of an AWS EFS file system:

* Storage Class:
  - EFS Standard (both Standard and Standard-IA).
    - Replicates data across multiple AZs (Multi-AZ).
  - EFS One Zone (both One Zone and One Zone-IA).
    - Replicates data within a single AZ.
* Performance Mode (set at EFS creation time):
  - General Purpose (default) - latency-sensitive use cases.
  - Max I/O - highly parallel, higher latency (big data, media processing).
    - cannot be used with EFS One Zone.
    - cannot be used with Elastic Throughput mode.
* Throughput Mode:
  - Bursting - throughput that grows with storage size.
  - Provisioned - throughput that is fixed.
  - Elastic - automatically scales throughput up or down based on workloads.

### Latency

* Both IA storage classes have a higher latency than their non-IA counterparts.

* Max I/O Performance mode has a higher latency, as it is designed for parallelized workloads, than the General Purpose (default) mode.

### Throughput

Throughput enforces a limit of the combined data size that can be read and written per second. It is not comparable to MB/s as there is no limit to data size read / written per second, while throughput is typically measured in percentage using throughput utilization (%).

The throughput of a file system cannot exceed a combined 100% of its read and write throughput. For example, if you EFS is using 33% of its read throughput limit, the EFS can simultaneously achieve up to 67% of its write throughput limit.

* Elastic Throughput mode scale out based on workloads performed.

* Provisioned Throughput mode is dependent on your specified throughput. If the throughput exceeds the Provisioned amount, then it automatically uses the Bursting Throughput allowed for the file system.

* Bursting Throughput mode scale out based on storage used.

### IOPS

* For EFS Standard and Standard-IA, both Elastic and Provisioned Throughput modes have higher IOPS than Bursting Throughput mode.

* For EFS One-Zone and One-Zone-IA, all three Throughput modes have the same throughput.

---
## References

* [EFS storage classes](https://docs.aws.amazon.com/efs/latest/ug/storage-classes.html)
* [Amazon EFS performance](https://docs.aws.amazon.com/efs/latest/ug/performance.html)