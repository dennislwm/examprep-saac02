<!-- TOC -->

<!-- TOC -->

- [Application Load Balancer ALB](#application-load-balancer-alb)
    - [Authentication Flow](#authentication-flow)
- [ASG Termination Policy](#asg-termination-policy)
    - [Default Termination Policy](#default-termination-policy)
- [References](#references)

<!-- /TOC -->

## Application Load Balancer (ALB)

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
## ASG Termination Policy

### Default Termination Policy

The default termination policy applies multiple termination criteria before selecting an instance to terminate. It first determines which AZs have the most instances, and it finds at least one instance that is not protected from scale in.

Within the selected AZ, the following termination criteria applies:

1. Any instances within an allocation strategy for On-Demand vs Spot instances.

2. Any instances that use the oldest launch template or launch configuration.

3. Any instances that are closest to the next billing hour.

4. If there are multiple above instances, terminate one at random.

---
## References

* [Authenticate users using an Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/listener-authenticate-users.html)
* [Work with Amazon EC2 Auto Scaling termination policies](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-termination-policies.html)