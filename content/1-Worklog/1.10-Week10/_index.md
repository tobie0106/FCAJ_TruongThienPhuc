---
title: "Week 10 Worklog"
date: 2026-06-22
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### 1. Objectives and Context

Week 10 focused on combining the separate labs into a more practical architecture flow. The group continued developing the project idea, received feedback from experienced members, and reviewed the roles of EC2, S3, IAM, and Storage Gateway. I also repeated the virtual-machine export workflow and practiced creating Storage Gateway, creating a file share, and mounting it from an on-premises client.

The objective was to validate a complete hybrid-storage flow: the on-premises machine accesses a file share, the gateway transfers data to S3, and IAM controls access to the bucket.

### 2. Worklog

### 2. Worklog

| No. | Task | Start date | End date |
|-----|------|------------|----------|
| 1 | Reviewed the requirements, objectives, and architecture of the group project | 22/06/2026 | 28/06/2026 |
| 2 | Received technical feedback and adjusted the roles of AWS services | 22/06/2026 | 28/06/2026 |
| 3 | Re-described the data flow from the on-premises environment to Amazon S3 | 22/06/2026 | 28/06/2026 |
| 4 | Repeated the virtual-machine or workload export process | 22/06/2026 | 28/06/2026 |
| 5 | Prepared the gateway host, S3 bucket, and IAM Role | 22/06/2026 | 28/06/2026 |
| 6 | Created and activated AWS Storage Gateway | 22/06/2026 | 28/06/2026 |
| 7 | Checked the gateway status before creating the file share | 22/06/2026 | 28/06/2026 |
| 8 | Created a file share connected to the destination S3 bucket | 22/06/2026 | 28/06/2026 |
| 9 | Configured access permissions and collected the mount information | 22/06/2026 | 28/06/2026 |
| 10 | Mounted the file share from the on-premises client and tested read/write operations | 22/06/2026 | 28/06/2026 |
| 11 | Checked the object, prefix, size, and update time in S3 | 22/06/2026 | 28/06/2026 |
| 12 | Reviewed the IAM Role and removed unused resources | 22/06/2026 | 28/06/2026 |

### 3. Improve the Project Architecture

During the discussion, the group described the operation flow instead of only listing services:

1. A user or client machine operates in the on-premises environment.
2. The client accesses a file share through a familiar file protocol.
3. Storage Gateway receives the file operation and transfers data to Amazon S3.
4. An IAM Role allows the gateway to access the required bucket scope.
5. EC2 or other components process the workload according to the project design.
6. The group validates permissions, connectivity, and data movement.

Technical feedback helped the group review which services were necessary, which permissions should be limited, and which validation steps were missing.

### 4. Storage Gateway and File-Share Process

#### Step 1: Prepare the resources

I reviewed the gateway host, network connectivity, destination bucket, and IAM Role. These components needed to be ready before creating the file share to avoid failures that would otherwise appear only during mounting or data transfer.

#### Step 2: Create and activate the gateway

I completed the activation process, assigned the gateway name, and reviewed its status. I continued to file-share creation only after the AWS Console showed that the gateway was operational.

#### Step 3: Create the file share

I selected the destination S3 bucket, configured access, and collected the mount information. I reviewed the client scope and the options that affected how files would become objects in S3.

#### Step 4: Mount from the on-premises client

On the client machine, I used the appropriate command or mount function for the file-share protocol. After mounting, I opened the directory, created a test file, and read it again to verify that file operations worked.

#### Step 5: Validate the object in S3

I opened the S3 Console and checked:

- Whether the object appeared in the bucket.
- Whether the key or prefix matched the expected directory structure.
- The object size and update time.
- Whether the gateway role had permissions broader than the required bucket scope.

### 5. Validation and Cause-Based Troubleshooting

| Item to validate | Possible cause | Validation method |
|------------------|----------------|-------------------|
| File share cannot be mounted | Incorrect path, network issue, or client not allowed | Review the mount command, connectivity, and share configuration |
| Share mounts but cannot be written | Insufficient IAM or share permission | Review the policy and error message |
| File does not appear in S3 | Gateway has not synchronized or uses the wrong bucket/prefix | Check gateway status and object location |
| Object appears in an unexpected path | Directory structure or prefix is incorrect | Compare the client path with the S3 object key |
| Resources continue generating cost | Instance, gateway, or bucket remains after the lab | Create a resource list and clean up in order |

### 6. My Contribution and Results

- Participated in reviewing the project architecture and data flow.
- Recorded technical feedback and updated the architecture description.
- Prepared and validated the components required by Storage Gateway.
- Created the file share and mounted it from the on-premises client.
- Tested read/write operations and compared the result with the S3 object.
- Reviewed the IAM Role according to the principle of least privilege.
- Removed resources that were no longer required.

### 7. Lessons Learned

Week 10 connected the knowledge from several previous weeks into one practical flow. Storage Gateway works correctly only when networking, the gateway, the file share, S3, and IAM are configured consistently. Validating each resource separately is not enough; the complete data path from the client to S3 must be tested.

I also gained a better understanding of architecture feedback. A diagram containing many services is not automatically a good design. A useful architecture must explain why each service exists, how access is controlled, how the system is tested, and how operational cost is managed.