# Chapter 7. EC2 Instance Storage

<!-- TOC -->

- [Chapter 7. EC2 Instance Storage](#chapter-7-ec2-instance-storage)
    - [EBS Volume](#ebs-volume)
        - [EBS Volume Type](#ebs-volume-type)
    - [EBS Security](#ebs-security)
    - [EBS Performance](#ebs-performance)
    - [EC2 Instance Store](#ec2-instance-store)
    - [Amazon EFS](#amazon-efs)
        - [EFS Storage Class](#efs-storage-class)
    - [EFS Security](#efs-security)
    - [EFS Performance](#efs-performance)
        - [Latency](#latency)
        - [Throughput](#throughput)
        - [IOPS](#iops)
    - [References](#references)

<!-- /TOC -->

---
## EBS Volume

* Think of EBS volume as a USB network drive, which can be detached from an EC2 instance and attached to another instance.

* `DeleteOnTermination` attribute is enabled on the root EBS volume, but not any other attached EBS volumes.

* Can copy EBS snapshot across AZ or Region, and restore it to another Region.

* EBS Snapshot Archive is an archive tier that is 75% cheaper, and restores at least 24 hours.

* Recycle bin for deleted EBS snapshots that can you can retain from 1 day to 1 year, in case of accidental deletion.

* For `io1`, `io2` volumes, you can Multi-Attach the same EBS volume with equal read and write permissions to multiple EC2 instances (up to 16 instances) within the same AZ. You must use a cluster-aware File System (not XFS, EXT4 etc).

### EBS Volume Type

There are two EBS volume types:

* Root EBS volumes:
  - General Purpose SSD (`gp2`, `gp3`) - volume that balances price and performance for a wide variety of workloads.
    - `gp3` has a baseline of 3,000 IOPS and throughput of 125 MiB/s, which can increase to 16,000 IOPS and 1,000 MiB/s throughput independently.
    - `gp2` has a baseline of 3,000 burst IOPS, where 3 IOPS per GB, and up to 16,000 burst IOPS with a 5,334 GB storage.
    Note: Burst IOPS increases based on storage size.
  - High Performance SSD (`io1`, `io2`) - volume for minimal latency and high throughput workloads, i.e. HPC.

* Non-root EBS volumes:
  - Low Cost HDD (`st1`) - volume for frequently accessed, throughput-intensive workloads.
  - Lowest Cost HDD (`sc1`) - volume for infrequent accessed workloads.

---
## EBS Security

Steps to encrypt an unencrypted EBS volume:

1. Create an unencrypted EBS snapshot of the volume.
2. Using COPY, encrypt the EBS snapshot.
3. Create a new encrypted EBS volume from the encrypted snapshot.
4. Attach the encrypted EBS volume to the original instance.

---
## EBS Performance

* You can use any of the standard RAID configurations for your EBS volumes as long as it is supported by the OS for your instance.

* Use RAID 0 when I/O performance is of the utmost importance, as you get the straight addition of throughput and IOPS when you add a volume.

* In RAID 0, I/O is distributed across the volumes in a stripe, and performance of the stripe is limited to the worst performing volume in the set.

* In RAID 0, the resulting size is the sum of the sizes of the volume, and the bandwidth is the sum of the available bandwidth of the volumes.

* In RAID 0, the loss of a single volume in the set results in a complete loss for the array.

---
## EC2 Instance Store

EC2 Instance Store has a higher performance than EBS volumes with better I/O performance. However, Instance Store is an ephemeral storage as they lose data if the instance is stopped.

It is primarily used to buffer cache temporary data for high-performance computation.

* Instace store volumes are attached only at instance launch, but not after launch.

* You cannot detach an instance store volume from one instance and attach it to another instance.

* You cannot configure an instance store volume to persist beyond the lifetime of its associated instance.

* The data persists only if the instance is rebooted, but does not persist if the instance is stopped, hibernated, or terminated.

* The data persists only if an instance store-backed AMI is created from the instance, but does not persist if a Windows AMI or EBS-backed AMI is created from the instance.

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

---
## EFS Security

* Data at rest encryption is supported when creating an EFS.
  - EFS uses either SSE-EFS (default free) or SSE-KMS for key management.
  - EFS metadata, i.e. file names, directory structure etc, are encrypted.
  - If you require audit or compliance of keys, use SSE-KMS for key management.
* Data in flight encryption is supported when you mount the EFS by enabling TLS.
* Use both IAM identity policies and resource policies to control access to an EFS.
  - The effective permission is the union of both identity and resource policies.
  - When an EFS has `PolicyNotFound`, the default file system policy grants full access to any anonymous client.
* Use a security group to authorize inbound and outbound access to an EFS.

---
## EFS Performance

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
* [Amazon EC2 instance store](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html)
* [Security in Amazon EFS](https://docs.aws.amazon.com/efs/latest/ug/security-considerations.html)