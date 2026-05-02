# IAM Privilege Escalation – Remediation

## Root Cause
User was granted:
- iam:AttachUserPolicy
- iam:CreateAccessKey
- Resource: "*"

This allowed unrestricted IAM changes.

---

## Risks
- Full account compromise
- Data exfiltration
- Infrastructure destruction
- Backdoor persistence

---

## Remediation Steps

### 1. Remove Dangerous Permissions
Avoid granting:
- iam:AttachUserPolicy
- iam:PutUserPolicy
- iam:CreateAccessKey
- iam:CreateUser
- iam:CreateRole
- iam:PassRole

---

### 2. Apply Least Privilege
Only grant necessary permissions.

---

### 3. Avoid Wildcards
Avoid:
- "Action": "*"
- "Resource": "*"

---

### 4. Use Explicit Deny

```json
{
  "Effect": "Deny",
  "Action": "iam:AttachUserPolicy",
  "Resource": "*"
}
```

---

### 5. Use IAM Roles
Prefer temporary credentials over long-term access keys.

---

### 6. Monitor IAM Activity
Track via CloudTrail:
- AttachUserPolicy
- CreateAccessKey
- CreateUser

---

### 7. Rotate Exposed Keys
- Delete compromised keys
- Create new ones if required
- Review logs for misuse

---

## Key Principle
Principle of Least Privilege

Users should only have permissions required for their role.