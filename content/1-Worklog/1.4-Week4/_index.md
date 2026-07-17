---
title: "Week 4 Worklog"
date: 2026-05-11
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### 1. Objectives and Context

During the fourth week, I moved from general AWS concepts to hands-on work with three foundational services: **Amazon EC2, Amazon S3, and AWS Identity and Access Management (IAM)**. The main objective was to understand how these services cooperate in a simple system: EC2 provides compute capacity for an application, S3 stores data or static files, and IAM controls identities and permissions.

The practical work required me to create resources, validate their status, deploy a basic application on EC2, and review the permissions required for users or services to access AWS resources without receiving unnecessary privileges.

### 2. Worklog

| No. | Task | Start date | End date |
|-----|------|------------|----------|
| 1 | Reviewed Amazon EC2, Amazon S3, and AWS IAM through documents and tutorial videos | 11/05/2026 | 17/05/2026 |
| 2 | Created and configured an Amazon EC2 instance according to the lab requirements | 11/05/2026 | 17/05/2026 |
| 3 | Checked the instance's `Running` state, status checks, VPC, subnet, and Security Group | 11/05/2026 | 17/05/2026 |
| 4 | Connected to the server and deployed a basic application on Amazon EC2 | 11/05/2026 | 17/05/2026 |
| 5 | Tested application access through the EC2 instance endpoint | 11/05/2026 | 17/05/2026 |
| 6 | Reviewed Security Group inbound rules and retained only the required ports | 11/05/2026 | 17/05/2026 |
| 7 | Studied the bucket and object structure used by Amazon S3 | 11/05/2026 | 17/05/2026 |
| 8 | Distinguished between IAM Users, IAM Policies, and IAM Roles | 11/05/2026 | 17/05/2026 |
| 9 | Documented the process for validating resources, applications, networking, and permissions | 11/05/2026 | 17/05/2026 |
| 10 | Continued self-study through videos about EC2, S3, and IAM | 11/05/2026 | 17/05/2026 |

### 3. Technical Process

#### Step 1: Prepare and create the EC2 instance

I opened the Amazon EC2 Console in the Region used for the lab, selected a configuration appropriate for the exercise, and launched an instance. During the creation process, I reviewed:

- The resource name for easier identification.
- The Amazon Machine Image and instance configuration required by the lab.
- The VPC, subnet, and network addressing assigned to the instance.
- The Security Group that controlled inbound traffic.
- The connection method required to configure the application.

After launching the instance, I waited until it reached the `Running` state and reviewed the status checks before continuing. This helped me understand that an instance appearing in the Console does not always mean it is fully ready; both the system and instance checks should be considered.

#### Step 2: Connect and deploy the application

After the instance became ready, I connected to the server using the method provided by the lab. I updated the environment, installed the components required by the application, transferred the application files, and started the service.

I then used the EC2 access endpoint to verify the deployment. When validating access, I reviewed the application process, the application port, the Security Group inbound rules, and the address used for testing. This made the relationship between operating-system configuration and AWS network configuration much clearer.

#### Step 3: Review the Security Group

I reviewed the inbound rules instead of allowing all traffic. Administrative access should be limited to the necessary source, while the application port should be opened only when required. This activity connected network configuration with the **principle of least privilege**, which applies not only to IAM but also to network access.

#### Step 4: Review S3 and IAM

For Amazon S3, I reviewed the bucket and object structure and how files are stored. For IAM, I distinguished between:

- **IAM User:** represents a person or account that needs to sign in.
- **IAM Policy:** defines which actions are allowed or denied on which resources.
- **IAM Role:** provides temporary permissions to users, applications, or AWS services.

I also learned why assigning a role to a service is normally safer than storing long-lived access keys in source code or on a server.

### 4. Validation and Technical Considerations

During this week, I focused on several conditions that can prevent a deployment from working correctly:

- Attempting to connect before the EC2 status checks are complete.
- Opening the wrong application port or allowing an unnecessarily broad source range.
- Uploading the application without starting its service.
- Using the wrong endpoint or confusing private and public addresses.
- Granting broader IAM permissions than the task requires.

Reviewing the deployment layer by layer — resource status, operating system, application, networking, and permissions — gave me a more systematic troubleshooting approach.

### 5. Results and My Contribution

- Created and validated an EC2 instance for the practical exercise.
- Completed the main steps required to deploy a basic application.
- Verified application access through the appropriate network configuration.
- Reviewed the Security Group and the reason for opening only required ports.
- Strengthened my understanding of buckets, objects, and data storage in Amazon S3.
- Distinguished more clearly between IAM Users, Policies, and Roles.
- Documented a validation process that can be reused in more complex labs.

### 6. Lessons Learned

Week 4 showed me that cloud deployment involves more than launching a server. A working system requires coordination between compute resources, the operating system, the application, networking, and permissions. If one layer is configured incorrectly, users may not be able to access the application even though the instance is running.

I also gained a stronger awareness of security during the initial configuration stage. Security Groups and IAM permissions should match the real requirement instead of being made unnecessarily broad just to complete a lab quickly. This became an important foundation for the Storage Gateway, CloudFront, and IAM policy activities in the following weeks.