# AWS Security & Encryption: KMS, SSM Parameter Store, Shield, WAF

<!-- TOC -->

- [AWS Security & Encryption: KMS, SSM Parameter Store, Shield, WAF](#aws-security--encryption-kms-ssm-parameter-store-shield-waf)
    - [AWS Managed vs AWS Owned vs Customer Managed Keys](#aws-managed-vs-aws-owned-vs-customer-managed-keys)
    - [KMS Multi-Region Keys](#kms-multi-region-keys)
    - [S3 Replication Encryption](#s3-replication-encryption)
    - [AMI Sharing Encryption](#ami-sharing-encryption)
    - [SSM Parameter Store Encryption](#ssm-parameter-store-encryption)
        - [Standard vs Advanced Tiers](#standard-vs-advanced-tiers)
        - [Parameter Policy](#parameter-policy)
    - [AWS Certificate Manager ACM](#aws-certificate-manager-acm)
        - [Request a Public Certificate](#request-a-public-certificate)
        - [Import a Public Certificate](#import-a-public-certificate)
        - [ACM Integration with ALB](#acm-integration-with-alb)
        - [ACM Integration with API Gateway](#acm-integration-with-api-gateway)
    - [AWS WAF](#aws-waf)
        - [AWS WAF using a Fixed IP](#aws-waf-using-a-fixed-ip)
    - [AWS GuardDuty](#aws-guardduty)
        - [GuardDuty Finding](#guardduty-finding)
        - [GuardDuty Finding Type](#guardduty-finding-type)
    - [References](#references)

<!-- /TOC -->

---
## AWS Managed vs AWS Owned vs Customer Managed Keys

|          AWS Managed Key           |                AWS Owned Key                 |      Customer Managed Key       |
|:----------------------------------:|:--------------------------------------------:|:-------------------------------:|
|      Required yearly rotation      |           Optional yearly rotation           |             Varies              |
|         Automatic rotation         |              Automatic rotation              |             Varies              |
|   Used only for one AWS account    |         Used for other AWS accounts          |  Used only for one AWS account  |
|     User cannot manage KMS key     |            User cannot manage key            |       User can manage key       |
|     User can view key metadata     |        User cannot view key metadata         |   User can view key metadata    |
|      Created by AWS services       | Created by AWS services in a service account |         Created by user         |
|    Per use fee, no monthly fees    |                     Free                     |    Per use fee, monthly fees    |
|   User can view CloudTrail logs    |       User cannot view CloudTrail logs       |  User can view CloudTrail logs  |
| Count towards KMS quota per second |   Does not count towards KMS quota per sec   | Count towards KMS quota per sec |

---
## KMS Multi-Region Keys

* A primary key is used to create replica keys across AWS regions, it is not a global key.

* Identical KMS keys with the same key ID, material, automatic rotation etc.

* Each key is managed independently in its Region.

* A use case is when data is encrypted in one region, transferred and decrypted into another region.
  - Encrypt DynamoDB Global tables to store sensitive data, such as PII.
  - Encrypt Aurora Global tables to store sensitive data, such as PII.

---
## S3 Replication Encryption

* SSE-S3 encrypted objects are replicated by default.

* SSE-C encrypted objects are never replicated.

* SSE-KMS encrypted objects will need to be enabled for replication.
  - Specify which target KMS key to encrypt S3 objects in the target S3 bucket.
  - Adapt the KMS key policy for the target KMS key.
  - An IAM role with `kms:Decrypt` permission for the source KMS key and `kms:Encrypt` permission for the target KMS key.
  - You may get KMS throttling errors due to KMS Quota limits, in which case you can ask for a Service Quota increase.

* You can use KMS Multi-Region keys for S3 Replication Encryption, but they are still treated as independent keys by AWS.
  - S3 objects will still be decrypted and then encrypted during the replication process.

---
## AMI Sharing Encryption

* A primary AMI is encrypted with a source KMS key from a source AWS account.

* Modify the AMI attribute to add a Launch permission that corresponds to a target AWS account.

* Modify the source Key policy so that you share the source KMS key with the target AWS account.

* Create a target IAM role/user in the target AWS account to have permissions for both shared AMI and key.

* When launching a target EC2 instance from the shared AMI, you can optionally specify a new KMS key to re-encrypt the attached volumes.

---
## SSM Parameter Store Encryption

* Optionally encrypt key-pair values using SSE-KMS.

* Can access Secrets Manager paths using a special prefix `/aws/reference/secretsmanager/SECRET_ID`.

* Can access AWS public prefixes for latest AMI IDs, e.g. `/aws/service/ami-amazon-linux-latest/AMI_ID`.

* Secured access through IAM.

### Standard vs Advanced Tiers

|    Standard Tier    |         Advanced Tier         |
|:-------------------:|:-----------------------------:|
|     10,000 max      |          100,000 max          |
|   4 KB size each    |        8 KB size each         |
| No parameter policy |   Parameter policy allowed    |
|      No charge      |         Charges apply         |
|    Free storage     | $0.05 per parameter per month |

### Parameter Policy

* Enable TTL (expiration) of a parameter to force an update for sensitive data, e.g. passwords.
  - Integrates with EventBridge to notify number of days prior to expiration date.

---
## AWS Certificate Manager (ACM)

* Easily provision, manage and deploy TLS certificates.

* Provide in-flight (transit) encryption for HTTPS scheme.

* Supports both free public TLS certificates and chargable private TLS certificates.

* Automatic TLS certificate renewals.

* Supports AWS resources.
  - ELB
  - CloudFront
  - API Gateway

* Not supported EC2 instances.

### Request a Public Certificate

1. List all domain names to be included in the certificate.
  - FQDN, such as `example.com`.
  - Wildcards, such as `*.example.com`.

2. Select validation method: DNS or Email Validation.
  - DNS Validation is preferred for automation.
  - Email Validation will send emails to contacts in the WHOIS database.
  - DNS Validation leverages on a CNAME record to DNS config in Route 53.
  - It will take a few hours to get verified.

3. The Public certificate will be enrolled for automatic renewal.
  - ACM will automatically renew certificates 60 days before expiry.

### Import a Public Certificate

* Optionally generate a Public certificate outside of ACM and then import it.

* No automatic renewal, must import a new certificate before expiry.

* ACM sends daily expiration events starting 45 days before expiration.
  - The number of days can be configured.
  - Events appear in EventBridge.

* AWS Config has a managed rule named `acm-certificate-expiration-check` to check for expiring certificates.

### ACM Integration with ALB

* ACM will provision and maintain TLS certs directly on ALB.

* ALB can enable HTTP to HTTPS redirect rule such that any HTTP request will automatically be sent over to HTTPS.

### ACM Integration with API Gateway

* ACM integrates best with Edge-Optimized Endpoint and Regional Endpoint for an API Gateway, but less so for Private Endpoint.
  - Create a Custom Domain Name in API Gateway.
  - Then setup a CNAME or an Alias record in Route 53.
  - For Edge-Optimized Endpoint, the ACM and TLS certificate must be in the same Region as CloudFront.
  - For Regional Endpoint, the TLS certificate must be imported into the API Gateway.

---
## AWS WAF

AWS WAF protects your web application from common Layer 7 (HTTP/S) exploits, but not Layer 4 (TCP/UDP).

AWS WAF can be attached to the following AWS resources:
* ALB (not NLB)
* API Gateway
* CloudFront
* AppSync GraphQL API
* Cognito User Pool

### AWS WAF using a Fixed IP

ALB does not support a fix IP address, but we can use Global Accelerator for a fixed IP to be used on a WAF.

* Users send requests to the Global Accelerator fix IP address.
* The traffic gets sent to the ALB with an attached WAF.
* The WAF filters traffic using the fix IP address, and passes any valid traffic to the ALB.

---
## AWS GuardDuty

* Intelligent threat discovery to protect your AWS account.
* Uses Machine Learning algorithms, anomaly detection, third-party data.
* One click to enable (30-day trial).
* AWS data sources includes:
  - CloudTrail Events logs
  - VPC Flow Logs
  - DNS Logs
  - EKS Audit Logs and runtime monitoring
  - RDS and Aurora login activity
  - EBS volumes
  - Lambda network activity
  - S3 Data Events and logs
* Can set up EventBridge rules to be notified in case of findings.
  - EventBridge rules can target AWS Lambda or SNS.
* Findings of the same type and target resource are dynamically updated with new information, without needing to generate a new finding or look through multiple similar reports.
* Findings of a different type or different resource are created with a unique finding ID.
* The aggregation criteria for each finding type are determined by AWS and cannot be configured.
* Can protect against CryptoCurrency attacks.
  - Has a dedicated finding for Cryptocurrency.

### GuardDuty Finding

A finding represents a potential security issue detected within your AWS environment. Each finding has an assigned severity level within the 1.0 to 8.9 range (values 0 and 9-10 are reserved for future use) with higher values indicating greater security risk. GuardDuty breaks down this range into High (7-8), Medium (4-6), and Low (1-3) severity levels.

### GuardDuty Finding Type

One of the most useful pieces of information in the finding details is a finding type. For example, the GuardDuty `Recon:EC2/PortProbeUnprotectedPort` finding type quickly informs you that an EC2 instance has an unprotected port that a potential attacker is probing.

The finding type has a naming convention of `ThreatPurpose:ResourceType/ThreatFamilyName.DetectionMechanism!Artifact`.

* Threat Purpose
  - Describes the primary purpose of a threat.
  - Values can be `Backdoor`, `Behavior`, `CredentialAccess`, `Cryptocurrency`, `DefenseEvasion` etc.
* Resource Type
  - Describes the potential AWS resource target.
  - Typical values can be EC2, S3, IAM, EKS etc.
* Threat Family Name
  - Describes the method in which GuardDuty detected the finding.
  - Values can be `DenialOfService`, `NetworkPortUnusual`, `PortProbeUnprotectedPort` etc.
* DetectionMechanism
  - Describes a variation of the method in which GuardDuty detected the finding.
  - Values can be `Tcp`, `Udp` etc, for example `DenialOfService.Tcp`.
* Artifact
  - Describes a specific resource owned by a tool that is used in the malicious activity.
  - Values can be `DNS` etc, for example `BitcoinTool.B!DNS` indicates a known Bitcon-related domain.

---
## References

* [AWS KMS concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html)
* [Understanding Amazon GuardDuty findings](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_findings.html)
* [GuardDuty finding format](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-format.html)