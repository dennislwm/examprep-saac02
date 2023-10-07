# Chapter 28. Disaster Recovery & Migrations

---
## Recovery Point Objective (RPO) vs Recovery Time Objective (RTO)

|                 RPO                 |               RTO                |
|:-----------------------------------:|:--------------------------------:|
| How far back in time to last backup | How much time to perform restore |
|  Measures amount data loss in time  |   Measures amount of down time   |
|   Higher RPO means more data loss   | Higher RTO means more down time  |

---
## DR Strategy

Ranked DR strategy based on fastest RTO:

* Multi (Hot) Site - expensive option, but minimal data loss and minimal down time.
* Warm Standby - less expensive option, but minimal data loss and minimal down time.
* Pilot Light - cheaper option, but short data loss and short down time.
* Backup and Restore - cheapest option, but high data loss and high down time.

---
## DR Tips

* Backup
  - EBS snapshots, RDS automated backups and snapshots.
  - Regular pushes to S3, lifecycle policy, cross-region replication.
  - From On-Premises: Snowball or Storage Gateway.

* High Availability
  - Use Route53 to migrate DNS over from Region to Region.
  - RDS Multi-AZ, ElastiCache Multi-AZ, EFS, S3.
  - Site-to-Site VPN as a recovery from Direct Connect.

* Replication
  - RDS Replication (Cross-Region), AWS Aurora + Global Databases.
  - Database replication from On-Premises to RDS.
  - Storage Gateway.

* Automation
  - CloudFormation, Elastic Beanstalk to re-create a whole new environment.
  - Recover / reboot EC2 instances with CloudWatch if alarms fail.
  - AWS Lambda functions for customized automations.

* Chaos
  - Netflix has a "simian-army" to randomly terminate production instances.

---
## Transferring Large Amount of Data into AWS

Example: Transfer 200 TB of data in the cloud over a 100 Mbps connection.

* Over the internet / Site-to-Site VPN:
  - Immediate setup.
  - Will take 185 days to transfer.

* Over Direct Connect (1 Gbps):
  - Long time to setup (1 Month).
  - Will take 18.5 days to transfer.

* Over Snowball:
  - Order 2 to 3 Snowballs.
  - Takes about 1 week for end-to-end transfer.
  - Can be combined with Database Migration Service (DMS) for database.

* On-going Replication / Transfers:
  - Site-to-Site VPN
  - DMS
  - DataSync

---
## Mitigating Failures for ElastiCache

When planning your ElastiCache implementation, you should plan so that failures have a minimal impact upon your application and data. 

There are two approaches that you can take to protect your application and data from failures when running the Redis engine:

* Redis Append Only Files (AOF)
* Redis Replication Group

### Redis AOF

When AOF is enabled, whenever data is written to your Redis cluster, a corresponding transaction record is written to a Redis Append Only File (AOF). You can run the AOF against a replacement cluster to repopulate it with data.

Some shortcomings of using Redis AOF:

* It is time-consuming, hence there is some downtime.
* The AOF can get big, hence the cost is high.
* Using AOF can't protect you from all failure scenarios, for example hardware fault.

### Redis Replication Groups

A Redis replication group is comprised of a single primary node which your application can both read from and write to, and from 1 to 5 read-only replica nodes. Whenever data is written to the primary node it is also asynchronously updated on the read replica nodes.

Benefits of Redis Replication Groups

* Managed DR, as a failed read replica will be automatically replaced with a new replica.
* Managed HA with Multi-AZ, as a failed primary will be replaced automatically by promoting a read-only replica node.

---
## References

* [Mitigating Failures](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/FaultTolerance.html)