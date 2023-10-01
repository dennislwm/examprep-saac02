# Chapter 13. Advanced Amazon S3

---
## S3 Lifecycle Rules with S3 Analytics

Lifecycle Rule has several components:

* Transition Action - configure objects to transition to another storage class.

* Expiration Action - configure objects to expire (delete) after some time.
  - Can be used to delete old versions of files (if versioning is enabled).

* Rules can be created for a bucket prefix, e.g. `s3://mybucket/prefix/*`.

* Rules can be created for a tag, e.g. `Department: Finance`.

S3 Analytics:

* helps you decide when to transition objects to another storage class.
  - does not work for One-Zone IA or Glacier.
  - only works for Standard and Standard IA.

* report is updated daily, 1 to 2 days to start seeing data analysis.

* good first step to put together Lifecycle Rules or improve them.

---
## S3 Requester Pays

In general, bucket owners will pay for all storage and data transfer costs associated with their bucket. However, you can enable Requester Pays, where any AWS authenticated user will pay for data transfer costs, while the owner always pays for storage costs.

---
## S3 Performance

S3 baseline performance:

* automatically scales to high request rates, latency 100-200 ms.

* achieve at least 3,500 PUT/COPY/POST/DELETE or 5,500 GET/HEAD requests per second per prefix per bucket.

* there are no limits to the number of prefixes in a bucket.

* if you spread reads across multiple prefixes, you can achieve higher requests per second.

S3 performance optimization:

* Multi-Part upload is recommended for files > 100 MB (must for files > 5 GB) to help parallelize uploads.

* Transfer Acceleration increases transfer speed by utilizing edge locations which will forward the data to the target region to minimize public internet hops.

* Byte-Range Fetches increases download speeds by parallelizing GETS to request specific byte ranges (similar to multi-part upload).

---
## References
