# Chapter 9. AWS Fundamentals: RDS + Aurora + ElastiCache

---
## Aurora

### High Availability

* Multiple AZs in a single Region.
* Synchronously replicates primary DB instance to six storage nodes.
* Can create up to 15 read-only Replicas.
* Can define a subset of Replicas as a Custom Reader Endpoint.
* During failover, Aurora automatically promotes a Replica to be the primary DB instance.
* Applications connect to cluster endpoint, to ensure a persistent connection during failovers.

For HA across multiple Regions, use Aurora Global tables.

### Failover

* Customize order of priorities, ranging from 0 to 15, for Replicas to be promoted with 0 as highest.
* If two or more Replicas share the same priorities, Aurora promotes the Replica with largest size.
* If two or more Replicas share the same size, Aurora promotes one Replica randomly.

### Multi-Master

* Every Aurora instance is a Writer Endpoint if you want HA for write operations.

### Aurora Global Database

* Useful for disaster recovery by having multiple Replicas across Regions.
* Aurora Global database has 1 primary region for Read/Write, and up to 5 secondary read-only regions.
  - Up to 16 Replicas per secondary Region.
  - Replication lag is less than one second.
  - Promotion of secondary Region has an RTO of less than one minute.

---
## Backup and Monitoring for RDS and Aurora

### RDS Backup

* Automated backups:
  - Daily full backup automatically during backup window.
  - Transaction logs are backed up every 5 minutes.
  - Point-in-time recovery from oldest backup to 5 minutes ago.
  - Retention from 1 to 35 days, 0 to disable automated backups.
* Manual backups:
  - Manually triggered by the user.
  - Retention of backups indefinitely.
  - If you stop an RDS you will still pay for storage. You should do a manual snapshot & restore to save on costs.

### Aurora Backup

* Automated backups:
  - Retention from 1 to 35 days, but cannot be disabled.
  - Point-in-time recovery from above timeframe.
* Manual backups are identical to RDS manual backups.

### Restore of RDS or Aurora Backups

* Restore a backup into a new Database.
* Restore a database from S3 bucket (On-Premises backups).
  - To restore an Aurora cluster from On-Premises backups, use Percona XtraBackup to initiate On-Premises backups into S3 bucket.

### Aurora Database Cloning

* Create a new Aurora DB Cluster from an existing one, usually create a test DB from a prod DB.
* Cloning uses a COPY-ON-WRITE protocol.
  - Initially, the new clone DB uses the same data volume as the original DB cluster (fast as no copying is needed).
  - When updates are made to the clone DB, then additional storage is allocated and copied to be separated from the original DB cluster.
* Very fast and cost-effective.

---
## RDS and Aurora Security

* Server-side encryption.
  - SSE-KMS must be defined at launch time.
  - If the master is unencrypted, then replicas cannot be encrypted.
  - To encrypt an unencrypted database, first create a snapshot and restore as encrypted.
* Data in transit.
  - TLS ready by default, use the AWS TLS root certificate client-side.
* Use IAM roles to connect to your database instead of username and password.
* Use security groups to control network access.
* No SSH available except for RDS Custom.
* Audit logs can be enabled and sent to CloudWatch Logs for longer retention period.

---
## RDS Proxy for RDS or Aurora

* Fully managed RDS Proxy in front of your RDS to allow apps to pool and share DB connections.
* Reduce database resources and minimize open connections and timeouts to improve DB performance.
* Serverless, autoscaling, high availability (multi-AZ).
* Reduce RDS and Aurora failover time by up to 66%.
* Supports RDS and Aurora.
* Enforce IAM authentication for DB, and securely store credentials in Secrets Manager.
* RDS Proxy is never public and must be accessed from VPC.

---
## ElastiCache Security

* IAM authentication is supported by Redis, but not Memcached.
* IAM policies are only used for API-level security.
* To use Redis authentication, set up a password/token at the Redis cluster level.
* You can attach a security group to control network access.
* SSL in flight encryption is supported.
* SASL-based authentication is supported by Memcached, but not Redis.

---
## Caching Strategies

There are two strategies you can implement for populating and maintaining your cache depending on the access patterns to your data: Lazy Loading and Write Through.

Both strategies have their benefits and shortfalls, but adding TTL to each strategy can mitigate their shortfalls.

* Lazy Loading (read updates)
  - data is cached only for requested data and when required.
  - all read data is cached, but data can become stale.
  - during a cache miss or has expired, data is fetched from the database.
  - node failures are not fatal for your application, though with increased latency.
  - cache misses have a noticable delay.
  - cache can become stale as there are no updates even when data is changed.
* Write Through (write updates)
  - data is cached whenever data is written to the database.
  - adds or update cache, when written to a database, no stale data.
  - no cache misses or has expired.
  - write requests have a noticable delay compared to read requests.
  - every write involves a write to the cache and a write to the database.
  - node failures are fatal, as missing data will not appeaer until data is written.
  - wasted resources as most data is never read.
* Adding TTL
  - For lazy loading, adding a TTL ensures that cache doesn't become stale.
  - For write through, adding a TTL ensures less wasted resources from data that is never read.
* Session Store
  - store temporary session data in cache using TTL features.
* Redis Sorted Set
  - guarantees both uniqueness and element ordering.
  - each time an element added, it is ranked in real time, and added in correct order.
  - use case is for a real time leaderboard.

---
## IAM Database Authentication for RDS

You can authenticate to your DB instance using IAM database authentication, which works with MariaDB, MySQL, and PostgreSQL using an authentication token. With this authentication, you don't need to use a password when you connect to a DB instance.

The authentication token is a short-lived credential that is managed externally usiing IAM. IAM database authentication provides the following benefits:

* Network traffic to and from the RDS is encrypted using SSL or TLS.

* Use IAM to centrally manage access to your DB resources, instead of managing access individually on each DB instance.

* Use profile credentials specific to your EC2 instance to access your database for greater security.

* Use for applications that create fewer than 200 connections per second.

---
## Reference

* [IAM database authentication for MariaDB, MySQL, and PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.html)
* [High availability for Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Concepts.AuroraHighAvailability.html)
* [Caching strategies](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Strategies.html)