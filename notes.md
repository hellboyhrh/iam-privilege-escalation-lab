# IAM Privilege Escalation Lab

## Overview
This lab demonstrates how a low-privileged AWS IAM user can escalate privileges due to a misconfigured IAM policy.

---

## Lab Environment
- Cloud provider: AWS
- Region: ap-southeast-2
- Admin user: cloud-lab-admin
- Test user: low-priv-user
- Logging: CloudTrail enabled

---

## Initial Permissions
The user `low-priv-user` was granted minimal permissions:
- s3:ListAllMyBuckets

---

## Vulnerability Introduced
A policy was attached with:
- iam:AttachUserPolicy
- iam:CreateAccessKey
- Resource: "*"

This allowed unrestricted IAM modifications.

---

## Key Concepts

### STS (Security Token Service)
Used to verify identity:

```bash
aws sts get-caller-identity
```

---

### ARN (Amazon Resource Name)
Unique identifier for AWS resources:

```
arn:aws:iam::123456789012:user/low-priv-user
```

---

### Least Privilege
Users should only have the minimum permissions required.

---

### Console vs API
AWS Console may block actions due to missing list permissions, but API/CLI can still allow them.

---

## Key Learning
A user with IAM modification permissions can escalate privileges even without admin access.