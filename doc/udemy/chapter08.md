# Chapter 8. High Availability and Scalability: ELB & ASG

<!-- TOC -->

- [Chapter 8. High Availability and Scalability: ELB & ASG](#chapter-8-high-availability-and-scalability-elb--asg)
    - [Elastic Load Balancer ELB](#elastic-load-balancer-elb)
        - [AZs and LB Nodes](#azs-and-lb-nodes)
        - [Cross-Zone Load Balancing](#cross-zone-load-balancing)
        - [Zonal Shift](#zonal-shift)
        - [Request Routing](#request-routing)
        - [Routing Algorithm](#routing-algorithm)
        - [Load Balancer Scheme](#load-balancer-scheme)
        - [Connection Draining](#connection-draining)
    - [ALB Authentication](#alb-authentication)
        - [Authentication Flow](#authentication-flow)
    - [Auto Scaling Group](#auto-scaling-group)
        - [Configure Instance Tenancy in a Launch Configuration or Launch Template](#configure-instance-tenancy-in-a-launch-configuration-or-launch-template)
    - [ASG Scaling Policy](#asg-scaling-policy)
        - [ASG Scaling Cooldown](#asg-scaling-cooldown)
    - [ASG Termination Policy](#asg-termination-policy)
        - [Default Termination Policy](#default-termination-policy)
    - [ASG Termination of Unhealthy Instances](#asg-termination-of-unhealthy-instances)
    - [References](#references)

<!-- /TOC -->


---
## Elastic Load Balancer (ELB)

ELB distributes incoming traffic across multiple EC2 instances. When combined with ASG, the two features allow you to create a system that automatically adds and removes EC2 instances in response to changing load.

An ELB accepts incoming traffic and routes requests to its registered targets, such as EC2 instances, in one or more AZs. The ELB also monitors the health of its registered targets and ensures that it routes traffic only to healthy targets.

You configure your ELB to accept incoming traffic by specifying one or more listeners. A listener is configured with a protocol, such as HTTP, and port number.

ELB supports the following types of load balancers:

* Application LBs
* Network LBs
* Gateway LBs
* Classic LBs

With Classic LBs, you register instances with the load balancer, while the other types of LBs register instances in target groups.

### AZs and LB Nodes

When you enable an AZ for your ELB, it creates a load balancer node in the AZ. If you register targets in an AZ, but do not enable the AZ, these registered targets do not receive traffic.

With an ALB, it is a requirement that you enable at least two or more AZs. If one AZ becomes unavailable or has no healthy targets, the load balancer can route traffic to the healthy targets in another AZ.

### Cross-Zone Load Balancing

When cross-zone load balancing is enabled, each load balancer node distributes traffic across the registered targets in all enabled AZs. For example, if AZ 1 has two registered targets, and AZ 2 has 8 registered targets, then each of the 10 targets receives 10% of the traffic.

When cross-zone load balancing is disabled, each load balancer node distributes traffic only across registered targets in its own AZ. Using the same example above, both load balancer nodes receives 50% traffic each, where each of the targets in AZ 1 receives 25% of the traffic, while each of the targets in AZ 2 receives 6.25% of the traffic.

With an ALB, cross-zone load balancing is always enabled at the load balancer level. At the target group level, cross-zone load balancing can be disabled.

### Zonal Shift

Zonal shift is a capability of Route 53 Application Recovery Controller (Route 53 ARC). With zonal shift, you can shift a load balancer resource away from an impaired AZ with a single action.

Zonal shifts are only supported on ALBs and NLBs with cross-zone load balancing turned off. You can't start a zonal shift for multiple AZs.

### Request Routing

Before a client sends a request to your load balancer, it resolves the load balancer's domain name using a DNS server. The DNS entry is controlled by Amazon, because your load balancers are in the `amazonaws.com` domain. The Amazon DNS servers return one or more IPs of the load balancer nodes to the client.

With NLB, a network interface is created for each AZ that you enable, to get a static IP address.

### Routing Algorithm

With ALBs, the load balancer node processes requests as follows:

1. Evaluates the listener rules in priority order to determine which rule to apply.

2. Selects a target from the target group for the rule action, using the configured routing algorithm for the target group. The default routing algorithm is round robin.

With NLBs, the load balancer node processes requests as follows:

1. Selects a target from the target group for the default rule using a flow hash algorithm. It bases the algorithm on:
  - protocol type
  - source IP and source port
  - destination IP and destination port
  - TCP sequence number

2. Routes each individual TCP connection to a single target for the life of the connection. The TCP connections from a client have different ports and sequence numbers, and can be routed to different targets.

### Load Balancer Scheme

When you create an ELB, you must choose either:

* Internal LB - load balancer nodes only have private IPs.

* Internet-facing LB - DNS name is resolvable to the public IPs of the load balancer nodes.\

Both types of LBs route requests to your targets' private IPs, so your targets do not need public IPs to receive requests from an internal or internet-facing LB.


### Connection Draining

This feature aims squarely at the intersection of activities of both ELB and ASG. While capacity management activities will take place from time to time, you may want to avoid breaking open network connections while taking an instance out of service, updating its software, or replacing it with a fresh instance that contains updated software.

You can avoid this situation by enabling Connection Draining for your ELB, and entering a timeout between one second and 1 hour. When Connection Draining is enabled, the process of deregistering an instance from an ELB gains an additional step.

For the duration of the configured timeout, the ELB will allow existing, "in-flight" requests made to an instance to complete, but it will not send any new requests to the instance. Once the timeout is reached, any remaining connections will be forcibly closed.

---
## ALB Authentication

The following authentication use cases are supported:

* through an identity provider (IdP) that is OpenID Connect (OIDC) compliant.

* through social IdPs, such as Facebook, Google etc, through the user pools supported by Amazon Cognito.

* through corporate identities, using SAML, OIDC, or OAuth, through the user pools supported by Amazon Cognito.

### Authentication Flow

The following workflow represents how an ALB uses OIDC to authenticate users:

1. User sends HTTPS request to a website hosted behind and ALB. When the conditions for a rule with an authentication action are met, the LB checks for an authentication session cookie in the request headers.

2. If the cookie is not present, the LB redirects the user to the IdP authorization endpoint so that the IdP can authenticate the user.

3. After the user is authenticated, the IdP sends the user back to the LB with an authorization grant code.

4. The LB presents the authorization grant code to the IdP token endpoint.

5. Upon receiving a valid authorization grant code, the IdP provides the ID token and access token to the ALB.

6. The ALB then sends the access token to the user info endpoint.

7. The user info endpoint exchanges the access token for user claims.

8. The ALB redirects the user with the `AWSELB` authentication session cookie to the original URI.

9. The ALB validates the cookie and forwards the user info to targets in the `X-AMZN-OIDC-*` HTTP headers set.

10. The target sends a response back to the ALB.

11. The ALB sends the final response to the user.

---
## ALB Support for Multiple TLS Certificates Using SNI

You can host multiple TLS secured applications, each with its own TLS certificate behind a single ALB. In order to use Server Name Indication (SNI), all you need to do is bind multiple certificates to the same secure listener on your ALB.

ALB will autoomatically choose the optimal TLS certificate for each client at no extra charge. The most common reason you might want to use multiple certificates is to handle different domains with the same ALB.

Wildcard certificates have some limitations in that it works for subdomains that match a simple pattern, while subject-alternate-name (SAN) certicates can support many different domains, but the same CA has to authenticate each one, which means that you have to reauthenticate and reprovision your certificate everytime you add a new domain.

### TLS vs SSL

SSL technically refers to a predecessor of the TLS protocol. TLS is a protocol for securely transmitting data, such as passwords etc, as it enables privacy, authentication, and integrity of the data being transmitted.

SSL / TLS provides encryption of data in transit. When a browser connects to your TLS-enabled ALB, ALB presents a certificate that contains your site's public key, which has been cryptographically signed by a CA, to ensure that the browser knows it's safe to use your site's public key to establish a secure connection.

---
## Auto Scaling Group

### Configure Instance Tenancy in a Launch Configuration or Launch Template

You can specify one of three tenancy options in a launch configuration (`host` not available) or launch template:

* Shared (`default`) - Your instances run on a shared physical hardware.

* Dedicated Instance (`dedicated`) - Your instances run on a single-tenant hardware.

* Dedicated Host (`host`) - Your instances run on a physical server with EC2 instance capacity fully dedicated to your use, an isolated server with configurations that you can control.

The following table summarizes the instance placement tenancy of the ASG instances launched in a VPC.

| Launch configuration tenancy | VPC tenancy = `default`  | VPC tenancy = `dedicated` |
|:----------------------------:|:------------------------:|:-------------------------:|
|        not specified         | shared-tenancy instances | single-tenancy instances  |
|          `default`           | shared-tenancy instances | single-tenancy instances  |
|         `dedicated`          | single-tenancy instances | single-tenancy instances  |

Note: You can change the tenancy of a launched instance from `dedicated` to `host`, or from `host` to `dedicated`, but not from or to `default`.

---
## ASG Scaling Policy

There are four types of Dynamic Scaling Policy:

- Target Tracking Scaling is the easiest to set up, e.g. average CPU to stay around 40%.

- Simple / Step Scaling when a CloudWatch alarm is triggered, e.g. CPU > 70% then add 2 units.

- Scheduled Actions anticipate a scaling based on known usage patterns, e.g. set capacity to 10 at 5pm every Friday.

- Predicitive Scaling continuously forecast load and schedule scaling ahead.

Some good metrics to scale on:
* CPU utilization to make sure your CPU utilization is constant.
* Request Count Per Target to make sure the number of requests per EC2 is constanct.
* Average Network In / Out to make sure your application traffic usage is constant.
* Any custom metrics that you push using CloudWatch.

### ASG Scaling Cooldown

After a scaling activity happens, you are in a cooldown period (default 300 seconds). During the cooldown period, the ASG will not launch or terminate additional instances to allow for metrics to stabilise.

Note: Use a custom AMI to reduce configuration time in order to be serving request faster and reduce the cooldown period.

---
## ASG Termination Policy

### Default Termination Policy

The default termination policy applies multiple termination criteria before selecting an instance to terminate. It first determines which AZs have the most instances, and it finds at least one instance that is not protected from scale in.

Within the selected AZ, the following termination criteria applies:

1. Any instances within an allocation strategy for On-Demand vs Spot instances.

2. Any instances that use the oldest launch template or launch configuration.

3. Any instances that are closest to the next billing hour.

4. If there are multiple above instances, terminate one at random.

---
## ASG Termination of Unhealthy Instances

ASG relies on EC2 or ELB health checks to determine the health state of an instance. 

ASG will terminate:
* stopping, stopped, shutting-down, and rebooted instances.
* instances with status check failure.
* instances that have enabled ELB health checks with health check failure.
* instances to match ASG capacity changes, such as min, max and desired.
* instances to rebalance instances in changed AZs.
* instances to balance distribution of On-Demand and Spot capacities.
* instances in response to a configured CloudWatch alarm.

Sometimes, you cannot determine why ASG didn't terminate an unhealthy instance. There could be several possible reasons:

* an instance health check grace period hasn't expired.
* ASG processes such as HealthCheck, ReplaceUnhealthy or Terminate has been suspended.
* ASG is in a cooldown period.
* an instance status is Impaired.
* an instance status is Insufficient Data.
* an instance status is Standby.
* an instance status is Terminating:Wait.
* an instance status is Terminating.
* an instance health check configuration set to EC2 that failed ELB health checks.
* an instance for which registration is still in progress.
* an instance in an AZ for which the ELB is not configured to route traffic to.
* an instance that hasn't passed the configured HealthyThreshold number of health checks.

---
## References

* [Authenticate users using an Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/listener-authenticate-users.html)

* [Work with Amazon EC2 Auto Scaling termination policies](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-termination-policies.html)

* [Why did Amazon EC2 Auto Scaling terminate an instance?](https://repost.aws/knowledge-center/auto-scaling-instance-how-terminated)

* [Why didn't Amazon EC2 Auto Scaling terminate an unhealthy instance?](https://repost.aws/knowledge-center/auto-scaling-terminate-instance)

* [ELB Connection Draining - Remove Instances From Service With Care](https://aws.amazon.com/blogs/aws/elb-connection-draining-remove-instances-from-service-with-care/)

* [Configure instance tenancy with a launch configuration](https://docs.aws.amazon.com/autoscaling/ec2/userguide/auto-scaling-dedicated-instances.html)

* [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)

* [Application Load Balancers Now Support Multiple TLS Certificates With Smart Selection Using SNI](https://aws.amazon.com/blogs/aws/new-application-load-balancer-sni/)