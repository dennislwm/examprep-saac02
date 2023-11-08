# Chapter 31. Security Standards

---
## Domain: Access control

The objective of this domain is to ensure that sufficient access management controls and formalized processes are in place so that the access to the organisation's assets and data by employees, contractors and third parties are only granted on the principle of least privilege, and managed in a controlled and consistent manner.

1. The organisation has implemented all the cybersecurity requirements to ensure that there are cybersecurity measures in place over who has access to the data and assets.

2. The organisation has established and implemented a privileged access solution that is appropriate and recognised in the industry to authenticate users and authorise access based on their roles to ensure that there is a more efficient and effective way of managing access.

### AWS IAM

* 1. You can centrally manage users, security credentials such as access keys, and permissions that control which AWS resources users and applications can access.

### AWS Identity Center

* 1. You can use multiple-account permissions to assign your workforce users access to AWS accounts. You can use application assignments to assign your users access to IAM Identity Center enabled applications, cloud applications, and customer SAML applications.

* 2. You can use IAM Identity Center for temporary elevated access management for both vendor-managed and supported solutions as well as self-managed and self-supported solutions.

---
## Control: Identify

1. Inventory - do you have visibility into the approved resources?

2. Logging - have you securely enabled all relevant logging?

### AWS Config

2. AWS Config rule for logging services:
- API Gateway: `api-gw-execution-logging-enabled`
- S3 Bucket: `s3-event-notifications-enabled`

---
## Control: Protect

1. Secure access management - does the service use least privilege practices?

2. Secure network configuration - does the service properly segment and isolate sensitive resources?

3. Data protection - does the service protect data?

4. API protection - does the service protect API operations?

5. Protective services - Are correct protective services in place and do they provide the correct amount of coverage?

6. Secure development - Do you use secure coding practices?

---
## Control: Detect

1. Detection services - Are the correct detection services in place and do they provide the correct amount of coverage?

### AWS Config

1. AWS Config rules for detection services:
- API Gateway: `api-gw-xray-enabled`
- Cloudformation Stack: `cloudformation-stack-notification-check`

---
## Control: Respond

1. Response actions - Do you respond to security events quickly and do you have security findings?

2. Forensices - Do you securely acquire forensic data and have you set up a forensic account?

---
## Control: Recover

1. Resilience - Does the service configuration support failovers, elastic scaling, and high availability, and have you established backups?

---
## Reference

* [The Cyber Security Agency (CSA) Cyber Trust mark certification](https://d1.awsstatic.com/whitepapers/compliance/CSA_Cyber_Trust_mark_certification_Cloud_Companion_Guide.pdf)

* [AWS Foundational Security Best Practices (FSBP) standard](https://docs.aws.amazon.com/securityhub/latest/userguide/fsbp-standard.html)

* [Control categories](https://docs.aws.amazon.com/securityhub/latest/userguide/control-categories.html)