# Chapter 25. Identity and Access Management (IAM) - Advanced

<!-- TOC -->

- [Chapter 25. Identity and Access Management IAM - Advanced](#chapter-25-identity-and-access-management-iam---advanced)
    - [AWS Organisations](#aws-organisations)
    - [IAM Conditions](#iam-conditions)
    - [IAM Roles](#iam-roles)
    - [IAM Roles vs Resource-Based Policies](#iam-roles-vs-resource-based-policies)
        - [Trust Policy](#trust-policy)
    - [IAM User vs IAM Role vs Resource-based Policy](#iam-user-vs-iam-role-vs-resource-based-policy)
    - [IAM Permission Boundaries](#iam-permission-boundaries)
        - [Use Cases](#use-cases)
    - [References](#references)

<!-- /TOC -->

---
## AWS Organisations

* A member account can only be part of one organization.

* The main account is the management account.

* Consolidated billing across all accounts.

* Shared reserved instances and Savings Plans discounts across all accounts.

* Root organisational unit (OU) with multiple organizational units, e.g. business units, project units etc.

* Send CloudWatch logs to a central logging account.

* SCPs applied to OU or member accounts, except management account. Must have an explicitly allow (by default deny).

* Deny has a precedence over Allow.

* Member accounts inherit SCPs from Root OU, and any parent OUs.

---
## IAM Conditions

* AWS Organization: use `aws:PrincipalOrgId` to allow or deny access to a resource for member accounts of a specific OU.

---
## IAM Roles

There are several different scenarios where you might use IAM roles:

* role as a proxy for a principal in the same account to access AWS resources.

* role as a proxy for a principal in another account to access AWS resources.

* role as a proxy for OIDC identities, such as Facebook, or iDP, such as Okta to access AWS resources.

IAM roles differs from IAM user as follows:

* temporary credentials that expires with IAM roles.

* trust policy that defines whcih conditions must be met to allow other principals to assume it.

* principal has to give up its existing permissions to inherit the IAM role's permissions.

---
## IAM Roles vs Resource-Based Policies

For cross-account, you can use either:

* Use an IAM role in Account B as a proxy (User A to assume a role in Account B) to access a resource in Account B.

* Attach a resource-based policy to a resource in Account B to allow access from User A from Account A.

When User A assumes a role in Account B, the user gives up all the original permissions in Account A, and take the permissions assigned to the role in Account B. For example, User A needs to scan a DynamoDB table in Account A and dump it in an S3 bucket in Account B.

When using a resource-based policy, User A does not have to give up the original permissions in Account A, when accessing a resource in Account B. For example, SNS, Lambda, S3 etc.

### Trust Policy

The trust policy defines which principals can assume the IAM role, and under which conditions. It is a specific type of resource-based policy for IAM roles.

In other words, a trust policy is a resource-based policy that is attached to an IAM role, instead of a resource. A trust policy has the same structure as other IAM policies, with Effect, Action and Condition components, while it has the same structure as a resource-based policy, with Principal but no Resource component.

Wildcards cannot be placed in the Principal component of a policy as part of an ARN. However, wildcards can be used in the Condition component. Also a trust policy cannot live on its own, it must be attached to an IAM role.

For example, the following trust policy would only allow the IAM role `LeeD` from the `111122223333` account to assume the role it is attached to.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::111122223333:role/LeeD"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

---
## IAM User vs IAM Role vs Resource-based Policy

|          IAM User          |          IAM Role          | Resource-based Policy                    |
|:--------------------------:|:--------------------------:|:-----------------------------------------|
|   Long lived credentials   |   Short term credentials   | No credentials                           |
|   Permission boundaries    |   Permission boundaries    | No boundaries                            |
|      No trust policy       |        Trust policy        | Resource-based policy (aka Trust policy) |
|     Cannot assume user     |        Assume role         | Cannot assume                            |
| Keeps original permissions | Loses original permissions | Keeps original permissions               |

---
## IAM Permission Boundaries

* IAM Permission Boundaries are supported for users and roles (but not groups).

* Use IAM Permission Boundaries to set the maximum permissions an IAM entity can get.

### Use Cases

* Delegate responsibilities to non-administrators within their permission boundaries, e.g. create new IAM users.

* Allow developers to self-assign policies and manage their own permissions, while making sure they cannot escalate their privileges.

* Useful to restrict one specific user instead of the whole AWS account.

---
## References

* [How to use trust policies with IAM roles](https://aws.amazon.com/blogs/security/how-to-use-trust-policies-with-iam-roles/)