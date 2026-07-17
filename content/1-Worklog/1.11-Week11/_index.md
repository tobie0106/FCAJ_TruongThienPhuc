---
title: "Week 11 Worklog"
date: 2026-06-29
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### 1. Objectives and Context

Week 11 was the integration and validation stage of the group project based on the agreed architecture. The main resources included **Amazon EC2, Amazon S3, IAM Roles, and AWS Storage Gateway**. Instead of confirming only that the resources existed, the group needed to validate access permissions, connectivity, data flow, service status, and resource cleanup.

My recorded work focused on reviewing the configuration, executing validation scenarios, comparing results, and preparing documentation for the final report.

### 2. Architecture and Validation Flow

The system flow was tested in the following order:

1. EC2 or the on-premises client has the required network connectivity.
2. Storage Gateway is operational.
3. The file share can be mounted from an approved client.
4. The IAM Role allows the gateway to access only the required S3 bucket.
5. A file created from the client appears as an object in S3.
6. Operations outside the approved scope are denied.
7. Unused resources are identified and removed.

Testing in this order helped the group locate failures in the compute, network, gateway, file-share, IAM, or S3 layer.

### 3. Worklog

| No. | Task | Start date | End date |
|-----|------|------------|----------|
| 1 | Checked the status of Amazon EC2, Amazon S3, and AWS Storage Gateway | 29/06/2026 | 05/07/2026 |
| 2 | Reviewed network configuration between EC2, the on-premises client, and the gateway | 29/06/2026 | 05/07/2026 |
| 3 | Reviewed the IAM Role trust relationship and permission policy | 29/06/2026 | 05/07/2026 |
| 4 | Compared the IAM Role permissions with the required bucket and prefix | 29/06/2026 | 05/07/2026 |
| 5 | Mounted the file share from an approved client machine | 29/06/2026 | 05/07/2026 |
| 6 | Listed the directory, created a file, wrote data, and read the content again | 29/06/2026 | 05/07/2026 |
| 7 | Confirmed that the file was synchronized as an object in Amazon S3 | 29/06/2026 | 05/07/2026 |
| 8 | Compared the file name, size, update time, and object key | 29/06/2026 | 05/07/2026 |
| 9 | Tested an invalid client or an IAM Role with insufficient permissions | 29/06/2026 | 05/07/2026 |
| 10 | Recorded the successful and denied validation results | 29/06/2026 | 05/07/2026 |
| 11 | Updated the architecture diagram, validation process, and project report | 29/06/2026 | 05/07/2026 |
| 12 | Reviewed, stopped, or deleted resources that were no longer required | 29/06/2026 | 05/07/2026 |

### 4. Detailed Validation

#### Test 1: Resource status

I checked the EC2 instance, gateway, and bucket before validating connectivity. If the instance or gateway was not ready, a failed mount could not be used to evaluate the policy or file-share configuration.

#### Test 2: IAM Role permissions

I reviewed the policy attached to the role and compared it with the bucket used by the project. The validation included:

- Whether the correct service could assume the role.
- Whether read/write access was limited to the required bucket or prefix.
- Whether the policy included unnecessary administrative actions.
- At which step an error appeared when permission was missing.

The objective was to make the system work without using a full-administrator policy.

#### Test 3: Mount and file operations

From the client, I mounted the file share and performed the following operations:

1. Listed the directory contents.
2. Created a new file.
3. Wrote data to the file.
4. Read the content again.
5. Checked the corresponding object in S3.

This scenario validated both connectivity and data-operation permissions.

#### Test 4: End-to-end data flow

I compared the file name, size, update time, and object key in S3. If the data did not appear correctly, I checked the path from the client back through the gateway instead of only refreshing the S3 Console.

#### Test 5: Denied scenarios

The group reviewed situations such as an unapproved client, insufficient role permissions, or operations outside the policy scope. The purpose of negative testing was to confirm that the system blocked invalid access instead of validating only the successful path.

### 5. Test Result Matrix

| Component | Scenario | Required result |
|-----------|----------|-----------------|
| EC2/Client | Connect to the gateway | Connection succeeds within the configured network scope |
| Storage Gateway | Check service status | Gateway is operational and can serve the file share |
| File Share | Mount and list the directory | An approved client can access it |
| IAM Role | Write an object to the designated bucket | Access is allowed only for the approved bucket scope |
| S3 | Compare the file and object | Name, size, and path are consistent |
| Cleanup | Review active resources | No unnecessary resources remain |

### 6. My Contribution

- Reviewed the status and configuration of resources involved in the validation flow.
- Examined the IAM Role policy and compared it with the S3 bucket scope.
- Performed file-share mounting, file creation, and S3 object verification.
- Recorded results from successful and denied scenarios.
- Updated the architecture and validation-process documentation.
- Participated in resource cleanup and cost review.

### 7. Cost Optimization and Cleanup

I created a list of resources used by the project and reviewed them before finishing:

- Stop or terminate EC2 instances that were no longer needed.
- Remove the gateway or file share after confirming that it was not required.
- Review S3 objects and bucket contents before deletion.
- Review roles and policies created only for the lab.
- Confirm that no task or resource remained active outside the project plan.

Cleanup was treated as part of the practical work rather than an optional step. In a cloud environment, an unused resource can continue generating cost even when nobody is actively using it.

### 8. Lessons Learned

Week 11 showed me the difference between “creating the resources” and “completing the system.” A system is complete only when the end-to-end data flow is validated, permissions are correctly limited, invalid cases are blocked, and resources are managed after use.

Documenting test cases and results also made the practical contribution clearer and helped the group identify causes when configurations changed.