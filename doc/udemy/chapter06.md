# Chapter 6. EC2 - SA Associate Level

<!-- TOC -->

- [Chapter 6. EC2 - SA Associate Level](#chapter-6-ec2---sa-associate-level)
    - [EC2 Placement Groups](#ec2-placement-groups)
    - [EC2 Hibernate](#ec2-hibernate)
    - [References](#references)

<!-- /TOC -->

---
## EC2 Placement Groups

You can define an EC2 placement strategy using placement groups. There are three types of strategies for placement groups:

* Cluster - clusters instances into a low-latency group in a single AZ.

* Spread - spreads instances across underlying hardware (max 7 instances per group per AZ) for critical applications.

* Partition - spreads instances across many different partitions (which rely on different sets of racks) within a single AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka).

---
## EC2 Hibernate

When we stop an instance, the data on EBS is kept intact in the next start. However when we terminate an instance, any EBS volumes (root) also set up to be destroyed is lost.

When we hibernate an instance, we achieve a new state:
* The in-memory (RAM) state is preserved (up to 150 GB).
* The instance boot is much faster.
* The RAM state is written to a file in the root EBS volume.
* The root EBS volume must be encrypted.
* The processes that were previously running on the instance are resumed.
* Previously attached data volumes are reattached.
* Instance retains its instance ID.

Use cases:
* Long-running processes.
* Saving the RAM state.
* Services that take a long time to start.
* Hibernation cannot be more than 60 days.
* Not charged for a hibernated instance.

---
## References

* []