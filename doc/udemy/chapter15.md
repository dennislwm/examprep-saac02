# Chapter 15. CloudFront & AWS Global Accelerator

<!-- TOC -->

- [Chapter 15. CloudFront & AWS Global Accelerator](#chapter-15-cloudfront--aws-global-accelerator)
    - [AWS Global Accelerator](#aws-global-accelerator)
        - [Unicast IP vs Anycast IP](#unicast-ip-vs-anycast-ip)
        - [Global Accelerator vs CloudFront](#global-accelerator-vs-cloudfront)
    - [Using AWS Global Accelerator to Achieve Blue/Green Deployments](#using-aws-global-accelerator-to-achieve-bluegreen-deployments)
    - [References](#references)

<!-- /TOC -->

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