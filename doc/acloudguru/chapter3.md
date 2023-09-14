# Chapter 3. Identity and Access Management (IAM)

<!-- TOC -->

- [Chapter 3. Identity and Access Management IAM](#chapter-3-identity-and-access-management-iam)
  - [Securing the Root Account](#securing-the-root-account)
  - [Assign Permissions Using IAM Policy Documents Consisting of JSON](#assign-permissions-using-iam-policy-documents-consisting-of-json)
  - [IAM Credentials](#iam-credentials)
  - [Lab 3.1. Introduction to AWS Identity and Access Management IAM](#lab-31-introduction-to-aws-identity-and-access-management-iam)
    - [Introduction](#introduction)
    - [Runbooks](#runbooks)
      - [Add the Users to the Proper Groups](#add-the-users-to-the-proper-groups)
      - [Use the IAM Sign-In Link to Sign In as Each User](#use-the-iam-sign-in-link-to-sign-in-as-each-user)
  - [Lab 3.2. Create and Assume Roles in AWS](#lab-32-create-and-assume-roles-in-aws)
    - [Create the Correct S3 Restricted Policies and Roles](#create-the-correct-s3-restricted-policies-and-roles)

<!-- /TOC -->

---
## Securing the Root Account

1. Navigate to IAM > Your Security Credentials > Activate MFA

2. Create an admin group for your administrators, and assign the appropriate permissions to this group.

3. Create user accounts for your administrators.

4. Add your users to the admin group.

## Assign Permissions Using IAM Policy Documents Consisting of JSON

Example of a Policy Document that could be assigned to one or more Users, Groups and/or Roles:

```json
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
```

> Note: Typically, you should not assign a Policy Document to Users, but rather to a Group, which should be categorized by functions, such as administrator, developer, etc. Roles are internal functions, such as EC2, that are within AWS.

A Policy Document can be either managed by AWS or created by a User.

## IAM Credentials

1. Manage policy documents on a Group level.

2. The Principle of Least Privilege

3. IAM is global as it does not apply to regions.

4. New users have no permissions when first created.

5. Access key ID and Secret Access Key for CLI are analogous to username and password for User.

6. Always set up password rotation policy.

7. **IAM Federation** allows you to use your existing account, typically Microsoft Active Directory, with AWS.

---
## Lab 3.1. Introduction to AWS Identity and Access Management (IAM)

### Introduction

You will focus on user and group management, as well as how to assign access to specific resources using IAM-managed policies. You will learn how to find the login URL, where AWS users can log in to their account.

### Runbooks

1. Add the Users to the Proper Groups.

2. Use the IAM Sign-In Link to Sign In as Each User.

<details>
<summary>Click here to start Lab 3.1.</summary>

#### 1. Add the Users to the Proper Groups

1. Navigate to the AWS Console > IAM > User groups.

2. Add `user-1` to the `S3-Support` group:
  - Select `S3-Support` > Users tab > click **Add users**.
  - From the list > check the box next to `user-1` > click **Add users**.

3. Add `user-2` to the `EC2-Support` group:
  - Select `EC2-Support` > Users tab > click **Add users**.
  - From the list > check the box next to `user-2` > click **Add users**.

4. Add `user-3` to the `EC2-Admin` group:
  - Select `EC2-Admin` > Users tab > click **Add users**.
  - From the list > check the box next to `user-3` > click **Add users**.

#### 2. Use the IAM Sign-In Link to Sign In as Each User

1. Navigate to AWS Console > IAM > Dashboard > in the AWS Account section, copy the sign-in URL.

2. Open a new browser tab, and navigate to the URL.

3. Log in to the AWS Console as `user1` with the password provided.

4. Navigate to S3 > click **Create bucket**.
  - For the **Bucket name**, enter a globally unique bucket name.

5. Leave all other default settings and click **Create bucket**.
  - You should receive an **Access Denied** error, indicating that your group policy is in effect.

6. Sign out of AWS Console > navigate to the sign-in URL.

7. Log in to the AWS Console as `user2` with the password provided.

8. Navigate to EC2 > select Instances (running) > select an EC2 instance > use the Instance state dropdown, then select **Stop instance**.
  - You should see an error message, since this user doesn't have the permissions to stop instances.

9. Sign out of AWS Console > navigate to the sign-in URL.

10. Log in to the AWS Console as `user-3` with the password provided.

11. Navigate to EC2 > select Instances (running) > select an EC2 instance > use the Instance state dropdown, then select **Stop instance**.
  - Verify that the instance is now stopped.

12. Sign out of the AWS Console.

</details>

---
## Lab 3.2. Create and Assume Roles in AWS

<details>
<summary>Click here to start Lab 3.</summary>

### Create the Correct S3 Restricted Policies and Roles

1. Create a Policy Document `S3RestrictedPolicy`.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:ListStorageLensConfigurations",
                "s3:ListAccessPointsForObjectLambda",
                "s3:GetAccessPoint",
                "s3:PutAccountPublicAccessBlock",
                "s3:GetAccountPublicAccessBlock",
                "s3:ListAllMyBuckets",
                "s3:ListAccessPoints",
                "s3:PutAccessPointPublicAccessBlock",
                "s3:ListJobs",
                "s3:PutStorageLensConfiguration",
                "s3:ListMultiRegionAccessPoints",
                "s3:CreateJob"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": "arn:aws:s3::336469381515:accesspoint/*"
        },
        {
            "Sid": "VisualEditor2",
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:*:336469381515:storage-lens/*",
                "arn:aws:s3:::cfst-3352-f6879ffebe26660840d3636d-appconfigprod1-nrqbybu5hf7t",
                "arn:aws:s3:::cfst-3352-f6879ffebe26660840d3636d-appconfigprod2-ucwno1m490ln",
                "arn:aws:s3:::*/*",
                "arn:aws:s3:*:336469381515:accesspoint/*",
                "arn:aws:s3:*:336469381515:job/*",
                "arn:aws:s3-object-lambda:*:336469381515:accesspoint/*",
                "arn:aws:s3:us-west-2:336469381515:async-request/mrap/*/*"
            ]
        }
    ]
}
```

2. Create a Role `S3RestrictedRole` with attached permissions above and trusted relationships below.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::336469381515:root"
            },
            "Action": "sts:AssumeRole",
            "Condition": {}
        }
    ]
}
```
</details>