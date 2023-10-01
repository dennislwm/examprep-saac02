# Chapter 16. AWS Storage Extras

<!-- TOC -->

- [Chapter 16. AWS Storage Extras](#chapter-16-aws-storage-extras)
    - [DataSync](#datasync)

<!-- /TOC -->

---
## DataSync

* Move large amount of data to and from:
  - On-premises to AWS (needs agent)
  - AWS to AWS (no agent required)

* Can synchronize to:
  - S3 (any storage class including Glacier)
  - EFS
  - FSx (Windows, Lustre, NetApp, OpenZFS etc)

* Replication tasks can be scheduled.

* File permissions and metadata are preserved (NFS, SMB etc).

* One agent task can use 10 Gbps, can setup a bandwidth limit.