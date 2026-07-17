---
title: "Week 7 Worklog"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### 1. Objectives and Context

Week 7 focused more deeply on **AWS Identity and Access Management**. Instead of reviewing only IAM concepts, I created an IAM User, Policy, and Role and then validated permissions in Amazon EC2. An important part of the lab was using **tag-based conditions** to control the creation or management of EC2 instances.

My objective was not only to confirm that an allowed operation worked, but also to test denied scenarios and verify that the policy enforced the intended restrictions.

### 2. Worklog

| No. | Task | Start date | End date |
|-----|------|------------|----------|
| 1 | Studied IAM Users, IAM Policies, and IAM Roles in greater detail | 01/06/2026 | 07/06/2026 |
| 2 | Created a separate IAM User for restricted-access testing | 01/06/2026 | 07/06/2026 |
| 3 | Signed in as the new IAM User and tested AWS Console access | 01/06/2026 | 07/06/2026 |
| 4 | Created an IAM Policy and reviewed `Action`, `Resource`, `Effect`, and `Condition` | 01/06/2026 | 07/06/2026 |
| 5 | Created an IAM Role and reviewed its trust relationship | 01/06/2026 | 07/06/2026 |
| 6 | Accessed Amazon EC2 in the specified Region as the restricted user | 01/06/2026 | 07/06/2026 |
| 7 | Tested an EC2 operation without the required tag | 01/06/2026 | 07/06/2026 |
| 8 | Tested an EC2 operation with an incorrect tag key or value | 01/06/2026 | 07/06/2026 |
| 9 | Tested an EC2 operation with the valid tag and compared the result | 01/06/2026 | 07/06/2026 |
| 10 | Created or attached a Restriction Policy for sensitive operations | 01/06/2026 | 07/06/2026 |
| 11 | Recorded `Allow`, `Deny`, and access-denied results | 01/06/2026 | 07/06/2026 |
| 12 | Removed EC2 instances and resources that were no longer required | 01/06/2026 | 07/06/2026 |

### 3. Technical Process

#### Step 1: Create a limited IAM User

I created an IAM User specifically for the lab instead of using an administrative account. After generating the sign-in information, I signed out and logged in as the new user to test permissions from the perspective of the restricted identity.

This was important because reviewing a policy from an administrator account does not reproduce the experience of the real user subject to the policy.

#### Step 2: Analyze the IAM Policy structure

When creating the policy, I focused on three main elements:

- **Action:** which AWS API operations are allowed or denied.
- **Resource:** which resources are covered by the policy.
- **Condition:** additional requirements, represented in this lab by request or resource tags.

I learned that a policy can allow a group of operations while still rejecting a request when the required tag condition is not satisfied. This provides more detailed control than granting access only at the service level.

#### Step 3: Create and review the IAM Role

I created an IAM Role and reviewed its trust relationship to identify which principal could assume it. I then reviewed the policies attached to the role and compared the role with an IAM User.

An IAM User has its own sign-in identity and commonly represents a person, while a role is assumed to receive temporary credentials. This reduces the need for long-lived access keys.

#### Step 4: Test EC2 operations with tag conditions

I opened the EC2 Console in the assigned Region and performed several validation scenarios:

1. Submit the operation without the required tag.
2. Submit it with an incorrect tag key or value.
3. Submit it with the valid tag.
4. Review the ability to view, create, or manage instances after the policy was applied.

I compared the allow or deny result with the policy logic. When an action was denied, I reviewed the access message and checked the action, resource, and condition instead of immediately granting broader permissions.

#### Step 5: Apply a Restriction Policy

I created or attached a policy that restricted operations that were not permitted in the lab environment. This clarified the effect of an explicit `Deny`, which can block an operation even when another policy contains an `Allow`.

After validation, I deleted the instances and other resources that were no longer required to avoid unnecessary cost.

### 4. Permission Test Matrix

| Scenario | Expected result | Purpose |
|----------|-----------------|---------|
| IAM User signs in and opens EC2 | Only approved functions are visible or usable | Validate basic access |
| Request does not include the required tag | Denied | Confirm that the condition is enforced |
| Request includes an invalid tag | Denied | Confirm that an incorrect tag cannot bypass the policy |
| Request includes the valid tag | Allowed within the policy scope | Validate the correct scenario |
| User attempts an operation covered by the Restriction Policy | Denied | Validate the security restriction |

### 5. Results and My Contribution

- Created an IAM User for restricted-access testing.
- Created and reviewed the structure of an IAM Policy.
- Created an IAM Role and examined its trust relationship.
- Tested EC2 access by signing in as the restricted user.
- Performed multiple tag-related scenarios instead of validating only a successful request.
- Improved my understanding of `Allow`, `Deny`, and `Condition` behavior.
- Cleaned up resources after the lab.
- Discussed with the group how authorization could be applied to the project.

### 6. Lessons Learned

Week 7 changed how I evaluate IAM permissions. Authorization should not be tested only by asking whether a user can complete a task. It should also confirm that the user is correctly blocked in invalid scenarios. A good policy must allow the required work while preventing operations outside the intended scope.

Tag-based conditions also showed me that tags are not used only for organization or cost allocation; they can become part of an access-control strategy.