# Chapter 13. Advanced Amazon S3

<!-- TOC -->

- [Chapter 13. Advanced Amazon S3](#chapter-13-advanced-amazon-s3)
    - [S3 Requester Pays](#s3-requester-pays)
    - [S3 Performance](#s3-performance)
        - [Tranfer Acceleration](#tranfer-acceleration)
        - [Transfer Acceleration vs CloudFront](#transfer-acceleration-vs-cloudfront)
    - [Filter Data Using S3 Select](#filter-data-using-s3-select)
    - [References](#references)

<!-- /TOC -->
Lifecycle Rule has several components:

* Transition Action - configure objects to transition to another storage class.

* Expiration Action - configure objects to expire (delete) after some time.
  - Can be used to delete old versions of files (if versioning is enabled).

You specify an S3 Lifecycle configuration as XML.

* Rules can be created for an object size, e.g. `ObjectSizeGreaterThan` or `ObjectSizeLessThan`.

* Rules can be created for a bucket prefix, e.g. `s3://mybucket/prefix/*`.

* Rules can be created for a tag, e.g. `Department: Finance`.

* Rules can be created for a combination of the above.

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

### Tranfer Acceleration

* You must enable Transfer Acceleration on the bucket, which will take up to 20 minutes before the bucket is optimized.

* You must use the tranfer acceleration endpoint, `bucket.s3-accelerate.amazonaws.com`, to connect to the enabled bucket.

* For each request, AWS automatically checks if the Transfer Acceleration is likely to be faster than a regular transfer, and if not, the request may bypass Transfer Acceleration and you will not be charged for that request.

* As a comparison to AWS Snowball, which has a typical 5-7 days turnaround time and storage size of up to 81 TB each, Transfer Acceleration can transfer up to 75 TB of data over a 1 Gbps line in that same time period.

* Transfer Acceleration cannot be used over Direct Connect, as it is best for submitting data over the public internet.

### Transfer Acceleration vs CloudFront

|            Transfer Acceleration            |                 CloudFront                  |
|:-------------------------------------------:|:-------------------------------------------:|
|             Uses edge locations             |             Uses edge locations             |
|             Uses an S3 endpoint             |         Uses a CloudFront endpoint          |
|               Non-cached edge               |                 Cached edge                 |
|    Same latency for subsequent requests     |    Lower latency for subsequent requests    |
| Higher throughput for large files (>= 1 Gb) | Better performance for small files (< 1 Gb) |

---
## Filter Data Using S3 Select

S3 Select uses SQL statements to filter and retrieve only a subset of data to reduce cost and latency. It works on objects stored in CSV, JSON, zipped CSV, zipped JSON or Apache Parquet format.

S3 Select results are in JSON format and the maximum length of a record is 1 MB. There are limitations on the storage class that you can use S3 Select for.

Similar to Byte Range Fetches in GET requests, S3 Select `ScanRange` parameter allows you to specify a range of bytes to query.

The difference between Byte Range Fetches and S3 Select ScanRange is that the former will return a fixed byte range, while the latter will return an extendable byte range depending on whether the last row or record within the range extends beyond the `ScanRange` parameter.

For example, consider a series or row in a CSV file.

```
A,B,C,D
E,F,G,H
```

If we specify a start at Byte 1 and end at Byte 4:

* Bye Range Fetch: `,B,C`
* Select ScanRange: `,B,C,D`

---
## References

* [Performance Guidelines for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/optimizing-performance-guidelines.html)
* [Filtering and retrieving data using Amazon S3 Select](https://docs.aws.amazon.com/AmazonS3/latest/userguide/selecting-content-from-objects.html)
* [Configuring fast, secure file transfers using Amazon S3 Transfer Acceleration](https://docs.aws.amazon.com/AmazonS3/latest/userguide/transfer-acceleration.html)
* [General S3 FAQs](https://aws.amazon.com/s3/faqs)