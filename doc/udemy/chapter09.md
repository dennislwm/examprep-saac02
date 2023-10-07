# Chapter 9. AWS Fundamentals: RDS + Aurora + ElastiCache

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