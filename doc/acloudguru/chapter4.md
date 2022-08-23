# Chapter 4. Identity and Access Management (IAM)

<!-- TOC -->

- [Chapter 4. Identity and Access Management IAM](#chapter-4-identity-and-access-management-iam)
  - [Securing the Root Account](#securing-the-root-account)
  - [Assign Permissions Using IAM Policy Documents Consisting of JSON](#assign-permissions-using-iam-policy-documents-consisting-of-json)
  - [IAM Credentials](#iam-credentials)
  - [Lab 4. Create and Assume Roles in AWS](#lab-4-create-and-assume-roles-in-aws)
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
## Lab 4. Create and Assume Roles in AWS

<details>
<summary>Click here to start Lab 4.</summary>

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