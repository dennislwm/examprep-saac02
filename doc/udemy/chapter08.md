# Chapter 8. High Availability and Scalability: ELB & ASG

<!-- TOC -->

- [Chapter 8. High Availability and Scalability: ELB & ASG](#chapter-8-high-availability-and-scalability-elb--asg)
    - [ALB Authentication](#alb-authentication)
        - [Authentication Flow](#authentication-flow)
    - [ASG Scaling Policy](#asg-scaling-policy)
        - [ASG Scaling Cooldown](#asg-scaling-cooldown)
    - [ASG Termination Policy](#asg-termination-policy)
        - [Default Termination Policy](#default-termination-policy)
    - [ASG Termination of Unhealthy Instances](#asg-termination-of-unhealthy-instances)
    - [References](#references)

<!-- /TOC -->

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