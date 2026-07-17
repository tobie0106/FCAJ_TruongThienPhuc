---
title: "Week 6 Worklog"
date: 2026-05-25
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### 1. Objectives and Context

Week 6 extended the S3 activities from the previous week. I focused on three areas: protecting the S3 origin by blocking public access, distributing content through **Amazon CloudFront**, and improving data protection through **S3 Versioning** and cross-Region object replication.

The objective was not only to create resources but also to validate the complete access flow: users retrieve content through CloudFront, S3 remains the origin, object versions are retained, and selected data can be replicated to a bucket in another Region.

### 2. Worklog

| No. | Task | Start date | End date |
|-----|------|------------|----------|
| 1 | Reviewed Amazon S3 configuration and enabled Block All Public Access | 25/05/2026 | 31/05/2026 |
| 2 | Tested the restriction of direct access to the S3 origin | 25/05/2026 | 31/05/2026 |
| 3 | Created an Amazon CloudFront distribution using S3 as the origin | 25/05/2026 | 31/05/2026 |
| 4 | Reviewed the origin, cache behavior, viewer protocol, and distribution domain | 25/05/2026 | 31/05/2026 |
| 5 | Accessed and validated content through the CloudFront domain | 25/05/2026 | 31/05/2026 |
| 6 | Updated content at the S3 origin and examined the effect of caching | 25/05/2026 | 31/05/2026 |
| 7 | Enabled S3 Bucket Versioning for the lab bucket | 25/05/2026 | 31/05/2026 |
| 8 | Uploaded multiple versions of the same object and reviewed Version IDs | 25/05/2026 | 31/05/2026 |
| 9 | Studied how to recover an overwritten or accidentally deleted object | 25/05/2026 | 31/05/2026 |
| 10 | Created a destination bucket in another Region and enabled Versioning | 25/05/2026 | 31/05/2026 |
| 11 | Configured the replication rule and IAM Role for Amazon S3 | 25/05/2026 | 31/05/2026 |
| 12 | Uploaded a new source object and checked the destination bucket | 25/05/2026 | 31/05/2026 |

### 3. Technical Process

#### Step 1: Restrict direct access to S3

After the static website activity, I re-enabled public-access blocking to study an architecture in which S3 was no longer used as a public website endpoint. The goal was to reduce direct origin access and move content delivery to CloudFront.

I reviewed previous public policies and settings to confirm that the new configuration did not grant broader access than necessary. This showed me that S3 permissions depend on the architecture: a bucket used as a public website endpoint is configured differently from a bucket used as a protected CloudFront origin.

#### Step 2: Create the CloudFront distribution

I created a CloudFront distribution and selected the S3 bucket as the origin. During configuration, I reviewed:

- The origin used to retrieve content.
- The method CloudFront used to access the origin.
- The default cache behavior.
- The viewer protocol used to access the distribution.
- The distribution domain used for validation.

After deployment completed, I accessed the object through the CloudFront domain and verified that it was returned correctly. I also reviewed the S3 configuration to confirm that restricting public access did not prevent CloudFront from retrieving the content through the configured access mechanism.

#### Step 3: Validate caching and content updates

When an object at the origin changes, CloudFront may continue returning a cached copy until the cache expires or is refreshed. This activity helped me understand the importance of cache duration, object naming, and invalidation when a new version must reach users quickly.

A CDN improves performance by reducing requests to the origin, but operators must also manage cache behavior when content changes.

#### Step 4: Enable S3 Versioning

I enabled Versioning and uploaded multiple versions of the same object. In the S3 Console, I displayed object versions and compared the Version IDs, update times, and current object state.

I also reviewed overwrite and deletion behavior. With versioning enabled, previous versions can remain available, and deleting an object may create a delete marker instead of immediately removing the complete history. This clarified how S3 can support recovery from operational mistakes.

#### Step 5: Configure cross-Region replication

I created a destination bucket in another Region and prepared a replication rule from the source bucket. Before creating the rule, I reviewed the required conditions:

- Versioning must be enabled on both source and destination buckets.
- S3 requires an IAM Role that can read source versions and write objects to the destination.
- The rule must define which objects are included.
- A new object should be used for validation after the rule becomes active.

After configuration, I uploaded a test object to the source and checked the destination bucket. This helped me distinguish automated replication from a manual copy operation: replication depends on a rule and the permissions assigned to the service.

### 4. Validation Points

- Testing the distribution before deployment completed.
- Using an incorrect origin or object path.
- Receiving an older object because it remained in CloudFront cache.
- Enabling versioning on only one bucket before replication.
- Assigning an IAM Role without the necessary source or destination permissions.
- Testing only an object that existed before the replication rule became active.

Documenting these conditions helped me understand why a resource can appear correctly configured but still fail to produce the expected result.

### 5. Results and My Contribution

- Changed the access model from public S3 access to CloudFront-based delivery.
- Created and validated a CloudFront distribution using an S3 origin.
- Improved my understanding of cache behavior during content updates.
- Enabled S3 Versioning and inspected multiple versions of the same object.
- Learned the role of Version IDs and delete markers in recovery.
- Created a destination bucket in another Region and practiced replication configuration.
- Reviewed versioning and IAM Role requirements before data replication.

### 6. Lessons Learned

Week 6 connected performance, security, and data recovery. CloudFront delivers content through an intermediate layer instead of exposing the origin directly. Versioning reduces the risk of accidental overwrite or deletion, while replication extends data protection to another Region.

I also learned that every feature has specific operational requirements. Deployment status, caching, versioning, and IAM permissions must all be validated; creating the resources alone is not enough.