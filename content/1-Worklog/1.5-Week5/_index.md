---
title: "Week 5 Worklog"
date: 2026-05-18
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### 1. Objectives and Context

The main focus of Week 5 was to build a hybrid storage flow between an on-premises environment and AWS through **AWS Storage Gateway**, and to practice hosting a static website with **Amazon S3**. I needed to understand how data created from an on-premises client could pass through a file share and become an object in S3.

I also practiced configuring public access for a static website. This activity helped me distinguish between making an entire bucket public and allowing read access only to the objects required by the website.

### 2. Worklog

| No. | Task | Start date | End date |
|-----|------|------------|----------|
| 1 | Studied the architecture and data flow of AWS Storage Gateway | 18/05/2026 | 24/05/2026 |
| 2 | Prepared the gateway machine, Amazon S3 bucket, and client machine for the lab | 18/05/2026 | 24/05/2026 |
| 3 | Created and activated AWS Storage Gateway through the AWS Console | 18/05/2026 | 24/05/2026 |
| 4 | Checked the gateway status and network connectivity | 18/05/2026 | 24/05/2026 |
| 5 | Created a file share connected to an Amazon S3 bucket | 18/05/2026 | 24/05/2026 |
| 6 | Configured access permissions and approved client information | 18/05/2026 | 24/05/2026 |
| 7 | Mounted the file share on the on-premises client and copied a test file | 18/05/2026 | 24/05/2026 |
| 8 | Confirmed that the test file appeared as an object in the S3 bucket | 18/05/2026 | 24/05/2026 |
| 9 | Created an S3 bucket and uploaded static website files | 18/05/2026 | 24/05/2026 |
| 10 | Enabled Static website hosting and configured object read permissions | 18/05/2026 | 24/05/2026 |
| 11 | Tested the website endpoint and reviewed Block Public Access and the bucket policy | 18/05/2026 | 24/05/2026 |
| 12 | Documented the results, difficulties, and validation process | 18/05/2026 | 24/05/2026 |

### 3. Technical Process

#### Step 1: Prepare the Storage Gateway flow

Before creating resources, I identified the main components of the lab:

- A gateway acting as the bridge between the on-premises environment and AWS.
- A file share that the client machine could access through a file protocol.
- An Amazon S3 bucket used as the backend storage location.
- IAM permissions that allowed Storage Gateway to access the required bucket resources.

Defining the data path in advance helped me understand the purpose of each step instead of only following instructions in sequence.

#### Step 2: Create and activate the gateway

I created the Storage Gateway according to the lab procedure, provided the required network information, and completed activation through the AWS Console. After activation, I checked the gateway status to confirm that it could communicate with AWS.

An important point was that the gateway needed stable network connectivity, access to the required endpoints, and the correct storage configuration. If the gateway was not operational, file-share creation and synchronization could not be validated correctly.

#### Step 3: Create the file share backed by S3

After the gateway became ready, I created a file share and selected the S3 bucket used as the backend. I reviewed access permissions, the share path, and the client information required for mounting.

From the on-premises client, I mounted the file share using the generated connection details. I then created or copied a test file into the mounted directory and opened the S3 Console to verify that a corresponding object appeared in the bucket. This test demonstrated that data from the client passed through the gateway and was stored in AWS.

#### Step 4: Host a static website on S3

I created an S3 bucket for website content and uploaded the files using the required directory structure. I enabled **Static website hosting**, selected the index document, and reviewed the website endpoint.

For access control, I considered two separate layers:

- The bucket-level **Block Public Access** settings.
- The bucket policy or object permissions required for read access.

Disabling public-access blocking alone was not sufficient. The website worked only when the required objects could be read through the correct policy, while write, delete, and administrative permissions remained protected.

### 4. Layered Validation

For Storage Gateway, I validated:

1. Gateway status in the AWS Console.
2. Network connectivity between the client and gateway.
3. File-share mount information.
4. Permissions to the S3 bucket.
5. Object creation after copying a test file.

For the S3 website, I validated:

1. The name and location of the index file.
2. Static website hosting configuration.
3. Block Public Access settings.
4. Bucket policy and object read permission.
5. The S3 website endpoint.

This approach helped me distinguish storage, permission, and file-path issues.

### 5. Results and My Contribution

- Created and activated Storage Gateway according to the lab.
- Created a file share connected to Amazon S3.
- Mounted the file share from an on-premises client and performed a test-file operation.
- Confirmed that data written through the file share appeared in the S3 bucket.
- Created an S3 bucket and uploaded static website content.
- Enabled static website hosting and tested the website endpoint.
- Improved my understanding of the difference between Block Public Access and object read permissions.
- Documented a validation sequence for future hybrid-storage labs.

### 6. Lessons Learned

Week 5 showed me that Storage Gateway is not simply another storage service. It is a bridge between familiar file-based access in an on-premises environment and the object-storage model of Amazon S3. Testing with an actual file made the data flow much clearer than reviewing the architecture only in theory.

The static website activity also improved my understanding of S3 security. Public website access should be configured carefully; broad bucket permissions are unnecessary when users only need read access to a defined set of website objects.