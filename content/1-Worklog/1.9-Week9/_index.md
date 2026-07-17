---
title: "Week 9 Worklog"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### 1. Objectives and Context

Week 9 continued the IAM topic with a stronger focus on organizing users into groups and validating permission limits. I created a **Limited IAM User**, tested allowed and denied operations, and practiced using IAM Groups, Policies, and Roles. The final topic introduced **AWS Key Management Service (AWS KMS)** and the role of encryption keys in data protection.

My objective was to build a scenario-based permission-testing approach and understand that encryption-key management also requires policies, usage permissions, and separate administrative responsibilities.

### 2. Worklog

| No. | Task | Start date | End date |
|-----|------|------------|----------|
| 1 | Created a Limited IAM User for permission testing | 15/06/2026 | 21/06/2026 |
| 2 | Signed in as the Limited IAM User and recorded accessible functions | 15/06/2026 | 21/06/2026 |
| 3 | Tested operations within the policy scope | 15/06/2026 | 21/06/2026 |
| 4 | Tested out-of-scope operations and recorded access-denied messages | 15/06/2026 | 21/06/2026 |
| 5 | Created an IAM Group and attached a policy | 15/06/2026 | 21/06/2026 |
| 6 | Added the IAM User to the group and tested effective permissions | 15/06/2026 | 21/06/2026 |
| 7 | Created an IAM Role and reviewed the trust policy | 15/06/2026 | 21/06/2026 |
| 8 | Distinguished the role trust policy from its permission policy | 15/06/2026 | 21/06/2026 |
| 9 | Created a key in AWS Key Management Service | 15/06/2026 | 21/06/2026 |
| 10 | Configured the alias, key administrators, and key users | 15/06/2026 | 21/06/2026 |
| 11 | Reviewed the KMS key policy and key usage permissions | 15/06/2026 | 21/06/2026 |
| 12 | Compared resource permissions with encryption-key permissions | 15/06/2026 | 21/06/2026 |

### 3. Test the Limited IAM User

After creating the user, I signed in through the IAM User sign-in path. I did not use an administrator account for validation because the objective was to observe the exact permissions of the restricted user.

I created two groups of scenarios:

- **Valid scenario:** an operation within the policy scope should succeed.
- **Invalid scenario:** an operation outside the scope or a sensitive action should be denied.

When an access-denied message appeared, I recorded the service, attempted operation, and related policy. Instead of assigning administrator access, I returned to the policy and identified the specific action that would actually be required.

### 4. Manage Permissions with IAM Groups

I created an IAM Group, attached a policy to the group, and added the user. I then signed in again to verify the effective permission change.

The activity showed several benefits of groups:

- Avoid attaching the same policy repeatedly to many users.
- Update permissions for a functional group in one place.
- Organize users according to their work responsibilities.
- Make permission reviews clearer.

I also learned that a user's effective permissions can come from several sources, such as directly attached policies, group policies, or permission boundaries. Therefore, the complete permission set must be considered during troubleshooting.

### 5. IAM Roles and Trust Relationships

When creating the role, I reviewed the trust policy to identify which service or principal could assume it. I then reviewed the permission policy to determine what the role could do after assumption.

The difference between the two policy types was important:

- A trust policy answers: **Who can use the role?**
- A permission policy answers: **What can the role do?**

If the trust policy is incorrect, the role cannot be assumed even when the permission policy includes the required actions.

### 6. Learn AWS KMS

I created a KMS key according to the lab and reviewed:

- The key type and intended scope.
- The alias used for easier identification.
- Key administrators and key users.
- The key policy controlling administrative and usage permissions.
- Integration with AWS services for data encryption.

This activity showed me that permission to access an object or resource does not automatically include permission to use its encryption key. When data is protected with KMS, the user or service also needs appropriate access to the key.

### 7. Test Matrix

| Scenario | Expected result | Validation purpose |
|----------|-----------------|--------------------|
| Limited user performs an approved operation | Success | Confirm the required permission is granted |
| Limited user attempts an out-of-scope operation | Access denied | Confirm the restriction is enforced |
| User is added to an IAM Group | Receives the group's effective permissions | Validate centralized management |
| Principal is not included in the trust policy | Cannot assume the role | Validate the trust relationship |
| User lacks the required KMS permission | Cannot use the key | Confirm the combined effect of key and IAM policies |

### 8. Results and Lessons Learned

- Created and tested a Limited IAM User with both successful and denied scenarios.
- Used an IAM Group to manage the user's permissions.
- Distinguished permission policies from trust policies.
- Created and reviewed the main components of a KMS key.
- Understood that access to encrypted data depends on both resource permission and permission to use the key.
- Improved my ability to read access-denied messages and trace them back to the relevant policy.

Week 9 showed me that AWS security is implemented through multiple layers. IAM controls identities and actions, while KMS adds control over encryption keys. Both layers must be considered when designing access or troubleshooting encrypted resources.