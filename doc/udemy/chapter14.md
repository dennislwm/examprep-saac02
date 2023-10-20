# Chapter 14. S3 Security

---
## S3 Encryption

You can encrypt data at rest in S3 buckets using one of 4 methods:
* Server Side Encryption (SSE)
  - SSE-S3 with AWS owned keys (default which is free).
  - SSE-KMS with AWS managed keys stored in KMS.
  - SSE-C with customer provided keys outside of AWS.
  - Encrypts only the object data, not the metadata.
* Client Side Encryption (CSE)
  - clients must encrypt data before sending data to S3 buckets.
  - must use HTTPS for sending data.
* Force encryption at rest using a resource policy in the S3 bucket.
  - `Condition.StringNotEquals.s3:x-amz-server-side-encryption: aws:kms`
  - this only applies to SSE-KMS and SSE-C
* SSE-S3 encryption is automatically applied to new objects in S3 buckets.
  - A resource policy is evaluated before the default encryption.

Encryption in transit:
* S3 exposes both HTTP and HTTPS endpoints.
* HTTPS is recommended and used by default.
* Force encryption in transit using a resource policy in the S3 bucket.
  - `Condition.Bool.aws:SecureTransport: false`

### SSE-S3

* Default option for encryption at rest is SSE-S3 which is free.
* Each object is encrypted with a unique key, which in turn is encrypted with a root key.
* Automatic rotation of root key.
* Uses 256-bit Advanced Encryption Standard (AES-256) to encrypt your data.
* S3 PUT request includes the `x-amz-server-side-encryption` header with a value equals to `AES256`.

### SSE-KMS

* Encryption at rest is provided through an integration of the AWS KMS service.
* View separate keys, edit control policies, and follow the keys in CloudTrail.
* Create and manage customer managed keys, or use AWS managed keys.
* S3 checksum as part of each object's metadata is stored in encrypted form.
* KMS keys must be in the same Region as the bucket.
* S3 uses the KMS features for envelope encryption, and stores the encrypted data key as metadata with the encrypted data.
* Envelope encryption means your data is encrypted using a data key, and then the data key is further encrypted with a KMS key.
* When an object is written to S3 bucket:
  - KMS generates a data key, encrypts it under the KMS key, and sends both the plaintext data key and the encrypted key to S3.
  - S3 receives both the plaintext data key and encrypted key.
  - S3 uses the former plaintext data key to encrypt the data, and removes the plaintext data key from memory.
  - S3 stores the latter encrypted key as metadata with the encrypted data.

### SSE-C

* Encryption at rest is managed with customer-provided keys.
* S3 manages the encryption as it writes to disks by applying AES-256 encryption to your data.
* Customer manages the encryption keys in an external key store.
* For a version-enabled bucket, each object version can have its own encryption key.
* S3 PUT request includes the `x-amz-server-side-encryption-customer-algorithm` header with a value of `true`.

---
## S3 Cross-Origin Resource Sharing (CORS)

### What is an Origin?

Origin is composed of a scheme (protocol), a host (domain) and a port (number). For example, `https://www.example.com` has a HTTPS scheme (protocol), a domain `www.example.com` and a implied port 443 (for HTTPS).

### What is CORS?

CORS is a web browser based security mechanism to allow or deny requests to other origins while visiting the main origin.

* Same origins have the same domain name and sub domain names, e.g. `https://example.com/app1` and `https://example.com/app2`.

* Different origins have the same domain name with different sub domains, e.g. `https://example.com` and `https://other.example.com`.

* Different origins have different domain names.

* The requests to other origins will not be fulfilled unless the other origins allows for the request using CORS headers, i.e. `Access-Control-Allow-Origin` and `Access-Control-Allow-Methods`.

* A use case is where the main origin may request an image to load from a different origin.

### S3 CORS

* We need to enable an S3 bucket with the correct CORS headers to allow for a specific origin or all origins (*).

* A use case is where the main origin (main S3 bucket) serves a static site that may request an image to load from a different origin (another S3 bucket).

---
## S3 MFA Delete

To use MFA Delete, Versioning must be enabled on the S3 bucket, and only the root account can enable/disable MFA delete.

MFA Delete must be enabled in order to perform the following tasks:
* Permanently delete an object version.
* Suspend versioning on the bucket.

---
## S3 Access Logs

* You may want to log any request made to S3, including denied, which is logged into another S3 bucket in the same AWS Region.

* Never set the logging bucket to be the monitored bucket as it will create a logging loop.

---
## S3 Pre-Signed URL

* Generate a pre-signed URL using the S3 console, CLI or SDK, for an S3 object in a private S3 bucket.

* Users given a pre-signed URL inherits the permissions of the user that generated the URL for GET/PUT method.

* Expiration of pre-signed URL.
  - S3 Console: 1 min up to 12 hours.
  - CLI/SDK: 3600 secs default, up to 604800 secs (~168 hours).

---
## Glacier Vault Lock

* Easily deploy and enforce compliance controls for S3 Glacier vaults with a Vault Lock Policy.

* Specify controls such as Write Once Read Many (WORM) in a Vault Lock Policy and lock the policy for future edits.

* After a Vault Lock Policy is locked, it cannot be changed or edited.

* While the policy is in the in-progress state (after you initiate the lock), you have 24 hours to validate your policy before completing the process. Otherwise your policy will be deleted.

A Vault Lock policy is different from a Vault Access policy. Both policies governs access controls to your vault. However a Vault Lock policy can be locked to prevent future changes, which provides strong enforcement for your compliance controls.

In contrast, you can use a Vault Access Policy to implement access controls that are temporary, and subject to change. You can use a combination of both policies. For example, you can implement a time-based data-retention rules in the Vault Lock policy (deny deletes), and grant read access to third parties (allow reads) in your vault access policy.

---
## S3 Object Lock

To use S3 Object Lock, Versioning must be enabled on an S3 bucket.

* Adopt a WORM model.

* Block an object version deletion for a specified amount of time.

* Compliance Retention mode (most strict).
  - Object versions cannot be overwritten or deleted by any user (including root user).
  - Object retention modes cannot be changed, and retention period cannot be shortened.

* Governance Retention mode (less strict).
  - Most users cannot overwrite or delete an object version or alter its lock settings.
  - Some users have special permissions to change the retention mode or delete the object.

* Set a Retention Period to protect the object for a fixed period, and it can be extended.

* Legal Hold can be placed independently of Retention mode to protect an S3 Object indefinitely, regardless of the retention period.
  - IAM users with `s3:PutObjectLegalHold` permission can freely place or remove Legal Holds.

---
## S3 Access Points

* You can attach a separate resource policy to one or more S3 bucket prefixes using an S3 access point.

* A use case may be to allow only finance users to access the S3 bucket prefix `/finance`.

* S3 access point resource policy takes precedence over the S3 bucket resource policy.

* Each Access Point has its own DNS name (either a public Internet Origin or a private VPC Origin).

* To use a private VPC Origin, you must create a VPC Endpoint (either Gateway or Interface).
  - The VPC Endpoint policy must allow access to the target S3 bucket and Access Point.

---
## S3 Object Lambda

* Use a Lambda function to pre-process an S3 object before it is retrieved by the caller application.

* To use S3 Object Lambda, you must create both an S3 Access Point and an S3 Object Lambda Access Point.

* A use case may be the caller application can access a redacted/enhanced data from an S3 object only.
  - Remove personally identifiable information for analytics.
  - Convert across data formats, such as XML to JSON.
  - Resize or watermark an image based on the parameters passed by the caller application.

* The S3 Object Lambda Access Point is used by the caller application to retrieve the S3 object.
  - The Object Lambda Access Point will invoke the Lambda function.
  - The Lambda function will retrieve the S3 object from the S3 Access Point.
  - The Lambda function will execute its code before passing the S3 object to the caller application.

---
## Reference

* [S3 Glacier Vault Lock](https://docs.aws.amazon.com/amazonglacier/latest/dev/vault-lock.html)
* [Protecting data with server-side encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/serv-side-encryption.html)
* [Using server-side encryption with Amazon S3 managed keys (SSE-S3)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingServerSideEncryption.html)
* [Using server-side encryption with AWS KMS Keys (SSE-KMS)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html)
* [Using server-side encryption with customer-provided keys (SSE-C)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerSideEncryptionCustomerKeys.html)