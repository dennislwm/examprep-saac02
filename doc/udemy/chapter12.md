# Chapter 12. Amazon S3

---
## S3 Replication

Replication enables automatic, asynchronous copying of objects across S3 buckets. Buckets that are configured for object replication can be owned by the same or different AWS accounts. You can replicate one to many S3 buckets across AWS Regions.

You can enable Cross-Region Replication (CRR) or Same-Region Replication (SRR) that uses live replication to automatically replicate new objects as they are written to the bucket, while S3 Batch Replication is used to replicate existing objects to a different bucket on-demand.

### CRR & SRR

* Must enable Versioning in source and destination buckets.
* Buckets can be in different AWS accounts, with proper IAM permissions to S3 buckets.
* Replication is asynchronous.
* After you enable replication, only new objects are replicated.
* Optionally use Batch Replication to replicate existing objects and objects that failed replication.
* Optionally can replicate delete markers, however deletions with a version ID are not replicated (to avoid malicious deletes).
* There is no chaining of replication, i.e. if bucket 1 has replication to bucket 2, which has replication to bucket 3, then objects created in bucket 1 are not replicated to bucket 3.

Use cases:
* CRR - compliance, lower latency access, replication across accounts.
* SRR - log aggregation, live replication between production and test accounts.

### S3 Batch Replication

S3 Batch Replication replicates existing objects to different buckets. Unlike live replication, these jobs can be run as needed.

* Replicate existing objects that were added before the Same-Region Replication or Cross-Region Replication were enabled.
* Replicate objects that previously failed to replicate with a replication status of FAILED.
* Replicate objects that were already replicated to newly added destinations.
* Replicate replicas objects that were created from a replication rule.

---
## References

* [Replicating objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication.html)