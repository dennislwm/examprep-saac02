# Chapter 15. CloudFront & AWS Global Accelerator

<!-- TOC -->

- [Chapter 15. CloudFront & AWS Global Accelerator](#chapter-15-cloudfront--aws-global-accelerator)
    - [CloudFront](#cloudfront)
        - [CloudFront - ALB or EC2 as an Backend Origin](#cloudfront---alb-or-ec2-as-an-backend-origin)
        - [Geo Restriction](#geo-restriction)
        - [Price Classes](#price-classes)
        - [Cache Invalidation](#cache-invalidation)
        - [High Availability with Backend Origin Failover](#high-availability-with-backend-origin-failover)
        - [Secure Backend Origin with Field-Level Encryption](#secure-backend-origin-with-field-level-encryption)
    - [AWS Global Accelerator](#aws-global-accelerator)
        - [Unicast IP vs Anycast IP](#unicast-ip-vs-anycast-ip)
        - [Global Accelerator vs CloudFront](#global-accelerator-vs-cloudfront)
    - [Using AWS Global Accelerator to Achieve Blue/Green Deployments](#using-aws-global-accelerator-to-achieve-bluegreen-deployments)
    - [References](#references)

<!-- /TOC -->

---
## CloudFront

### CloudFront - ALB or EC2 as an Backend Origin

Users can access an ALB or EC2 instance from the internet through a CloudFront Edge Location:

* EC2 instance must be public.
  - Security group of EC2 must allow list of Public IPs of Edge Locations.

* ALB must be public, while EC2 instances behind ALB can be private.
  - Security group of EC2 must allow security group of ALB.

### Geo Restriction

* You can restrict who can access your distribution:
  - Allow list: allow a list of approved countries.
  - Block list: block a list of banned countries.

* The "country" of a incoming traffic is determined using a third-party Geo-IP database.
  - Users can still use a VPN to spoof their IP address.

* Use case: Control access to content based on copyright laws.

### Price Classes

* The cost of outgoing traffic (egress) per Edge Location varies.

* The cost of egress data decreases per unit as the volume increases.

* You can reduce the number of Edge Locations for cost reduction:

* There are three Price Classes:
  - Class All - all regions, including class 200 and rest of South America, Australia, NZ.
  - Class 200 - most regions, except the most expensive regions, includes class 100 and Asia, Africa/Middle East, Sydney, Brazil.
  - Class 100 - least expensive regions, i.e. North America and Europe.

### Cache Invalidation

When you update the backend origin, CloudFront does not know about it until the TTL has expired and it gets the refreshed content. However, you can force a partial (`/images/*`) or full (`*`) cache refresh (thus bypassing the TTL) by performing a CloudFront Cache Invalidation to delete the cache in the Edge Location and it will get the new content the next time a user request your data.

### High Availability with Backend Origin Failover

You can set up HA by creating an origin group with both a primary and a secondary backend origins. Then you create or update a cache behaviour to use the origin group.

CloudFront will only send request to the secondary origin after a request to the primary origin fails.

### Secure Backend Origin with Field-Level Encryption

You can enforce end-to-end connections to backend origins by using field encryption together with HTTPS. Field-level encryption provides encryption of sensitive data at the edge, close to the user, and remains encrypted throughout your entire application stack.

You can encrypt up to 10 individual data fields in a POST request, but you cannot encrypt all the data. Field encryption uses asymmetric encryption (public and private key pairs), and you must provide the public key to CloudFront.

---
## AWS Global Accelerator

Use case:

* Application deployed in a single region with global users who want to access it.

* Public internet can add a lot of latency to users outside the region due to many hops.

* Wants to go as fast as possible through AWS internal network with minimal latency.
  - Traffic routes to AWS edge location of a region within AWS internal network to minimize hops, then routes to end user using public Internet.
  - Anycast IP to route traffic based on nearest server.

Features:

* Works with Elastic IP, EC2 instances, ALB, NLB, public or private.

* Consistent performance:
  - Intelligent routing to lowest latency and fast regional failover.
  - No issue with client cache (because the IP doesn't change).
  - Internal AWS network.

* Health checks of your applications.

* Security:
  - Only two external IPs need to be whitelisted.
  - DDoS protection thanks to AWS Shield.

### Unicast IP vs Anycast IP

|         Unicast IP         |               Anycast IP               |
|:--------------------------:|:--------------------------------------:|
|  One server holds one IP   |      All servers hold the same IP      |
| Traffic routed based on IP | Traffic routed based on nearest server |

### Global Accelerator vs CloudFront

|                         Global Accelerator                         |                             CloudFront                             |
|:------------------------------------------------------------------:|:------------------------------------------------------------------:|
|                      Use AWS internal network                      |                      Use AWS internal network                      |
|                   AWS Shield for DDoS protection                   |                   AWS Shield for DDoS protection                   |
| Proxying packets at edge locations to server, no caching available |  Improves performance for both cacheable content, such as images   |
|       Good for non-HTTP traffic, such as gaming (UDP) or TCP       | Improves performance for dynamic content, such as API acceleration |
|                                Content is served from server                                 |                 Content is served at edge location                 |

---
## Using AWS Global Accelerator to Achieve Blue/Green Deployments

Blue/green deployment is a technique for releasing applications by shifting traffic between two identical environments running different versions of the application. Blue is the current running version and green the new version. Blue/green deployment can mitigate common risks associated with deploying software, such as downtime and rollback capability.

AWS Global Accelerator uses endpoint weights to determine the proportion of traffic that is directed to endpoints in an endpoint group (an AWS region where your application is deployed), and traffic dials to control the percentage of traffic that is directed to an endpoint group.

Many customers use DNS weighted routing (using Route 53 weighted routing) to implement blue/green deployments. However, this option may not fit the use-cases that require a fast and controlled transition of the traffic. Some client devices and internet resolvers cache DNS traffic across the internet, and the downside is that you don't know how long it will take before all of your users receive updated IP addresses when you update a record, change your routing preferences or when there is an application failure.

With AWS Global Accelerator, you can shift traffic gradually or all at once between the blue and the green environment and vice-versa without being subject to DNS caching on client devices or internet resolvers, traffic dials and endpoint weight changes are effective within seconds. In addition, you get two static anycast IP addresses that provide a fixed entry point to your applications. This lets you easily move your infrastructure between AZs or regions, without having to update the DNS configuration or client-facing applications.

---
## References

* [Using AWS Global Accelerator to achieve blue/green deployments](https://aws.amazon.com/blogs/networking-and-content-delivery/using-aws-global-accelerator-to-achieve-blue-green-deployments/)