---
title: "Week 8 Worklog"
date: 2026-06-08
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### 1. Objectives and Context

During Week 8, I combined two areas of work: reviewing the AWS architecture for the group project and studying the process of moving a virtual machine from an on-premises environment to AWS. The practical activity used **VMware Workstation** to prepare the virtual machine and introduced the export, upload, and import preparation stages.

The objective was to understand that migration is not simply copying a file. The virtual-machine format, operating system, storage size, upload location, IAM Role, and image compatibility all need to be considered before a workload can run in the cloud.

### 2. Worklog

| No. | Task | Start date | End date |
|-----|------|------------|----------|
| 1 | Consolidated knowledge about compute, storage, security, and migration | 08/06/2026 | 14/06/2026 |
| 2 | Reviewed the project objective, components, and data flow with the group | 08/06/2026 | 14/06/2026 |
| 3 | Identified the on-premises components and AWS components | 08/06/2026 | 14/06/2026 |
| 4 | Installed and became familiar with VMware Workstation | 08/06/2026 | 14/06/2026 |
| 5 | Started and validated the virtual-machine operating system | 08/06/2026 | 14/06/2026 |
| 6 | Reviewed the VM CPU, memory, disk size, and network configuration | 08/06/2026 | 14/06/2026 |
| 7 | Shut down the virtual machine correctly and prepared it for export | 08/06/2026 | 14/06/2026 |
| 8 | Exported the virtual machine according to the lab instructions | 08/06/2026 | 14/06/2026 |
| 9 | Reviewed the generated files, file format, and total size | 08/06/2026 | 14/06/2026 |
| 10 | Studied how to upload virtual-machine files to Amazon S3 | 08/06/2026 | 14/06/2026 |
| 11 | Reviewed the IAM Role, trust policy, and S3 permissions for VM Import/Export | 08/06/2026 | 14/06/2026 |
| 12 | Documented compatibility requirements and import-task monitoring steps | 08/06/2026 | 14/06/2026 |

### 3. Project Architecture Review

During the group discussion, we reviewed:

- The problem that the project needed to solve.
- Which components remained on premises.
- Which components were deployed on AWS.
- The direction in which data moved.
- The services responsible for compute, storage, and access control.
- How the system would be tested after deployment.

Discussing the architecture through the data flow helped the group avoid selecting services only because we had previously studied them. Each service needed a clear purpose in the design.

### 4. Virtual Machine Preparation and Export

#### Step 1: Validate the VMware environment

I installed VMware Workstation, opened the virtual machine used for the exercise, and reviewed:

- Whether the operating system started correctly.
- Virtual disk size and total file size.
- CPU, memory, and network configuration.
- The configuration and disk files associated with the VM.

Before exporting, I shut down the virtual machine correctly to reduce the risk of inconsistent disk data.

#### Step 2: Export the virtual machine

I used the VMware export function to create a portable set of virtual-machine files according to the lab instructions. After export, I reviewed the storage location, total size, and generated files.

This validation was important because uploading an incomplete or corrupted set of files could cause the import task to fail later.

#### Step 3: Prepare the AWS upload

I studied the process of using S3 to store the VM files before import. The main components included:

- An S3 bucket containing the image or disk file.
- An IAM Role that allowed the import/export service to read the S3 object and create the required resources.
- Container description or source information used by the import task.
- The Region in which the bucket and import task were executed.

I learned that uploading the files to S3 is only a preparation step. The import task must still be monitored, and the resulting image must be validated after completion.

### 5. Technical Considerations

- Do not export while the virtual machine is running or improperly shut down.
- Review the format and size before uploading.
- Ensure that the bucket and import task use an appropriate Region.
- The IAM Role requires both the correct trust policy and limited S3 permissions.
- The operating system and VM configuration must be supported.
- A successful upload does not guarantee a successful import; the task status must be monitored.

### 6. Results and My Contribution

- Consolidated information from previous sharing sessions and labs.
- Participated in reviewing the architecture and data flow of the group project.
- Installed and used VMware Workstation for the simulated on-premises environment.
- Reviewed the virtual-machine configuration before export.
- Produced a set of VM files for the upload stage.
- Understood the roles of S3 and IAM in VM Import/Export.
- Documented the conditions that should be checked before importing a workload into AWS.

### 7. Lessons Learned

Week 8 showed me that migration is a multi-stage process with several dependencies. An unsupported format, missing file, incorrect IAM permission, or unsupported operating-system configuration can prevent a successful import.

I also improved how I discussed architecture with the group. Instead of listing many services, the design should explain what problem each component solves and how data moves through the system.