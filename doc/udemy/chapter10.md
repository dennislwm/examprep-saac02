# Chapter 10. Route 53

<!-- TOC -->

- [Chapter 10. Route 53](#chapter-10-route-53)
    - [CNAME vs Alias](#cname-vs-alias)

<!-- /TOC -->

* Alias - points a root or non-root domain to an AWS hostname. For example, `example.com` points to `myweb.us-east-2.s3.amazonaws.com`.

Aliases are an AWS Route 53-specific extension to DNS server functionality. Unlike CNAME, it can be used for the top node of a domain name (Zone Apex), e.g. `example.com`.

|                        Alias                         |                   CNAME                    |
|:----------------------------------------------------:|:------------------------------------------:|
|               redirect to AWS resource               |         redirect to any DNS record         |
|             can create at the zone apex              |       cannot create at the zone apex       |
|                  cannot set the TTL                  |              can set the TTL               |
| respond to query when name and type of alias matches | respond to query regardless of record type |
|             No charge for alias queries              |         Charges for CNAME queries          |