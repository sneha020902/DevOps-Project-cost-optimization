# 🧹 AWS Cloud Cost Optimization — Stale EBS Snapshot Cleanup

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![AWS Lambda](https://img.shields.io/badge/AWS_Lambda-FF9900?style=flat-square&logo=awslambda&logoColor=white)
![Amazon EC2](https://img.shields.io/badge/Amazon_EC2-FF9900?style=flat-square&logo=amazonec2&logoColor=white)
![Boto3](https://img.shields.io/badge/Boto3-232F3E?style=flat-square&logo=amazon&logoColor=white)

---

## 🧩 The Problem

AWS accounts accumulate stale EBS snapshots over time — snapshots whose original volumes or instances no longer exist. These sit silently in your account, costing money every month without providing any value. Manually hunting them down is tedious and error-prone.

---

## ✅ The Solution

A serverless AWS Lambda function written in Python (Boto3) that automatically:
- Scans all EBS snapshots owned by the account
- Checks if each snapshot's volume still exists and is attached to a running instance
- Deletes any snapshot that is stale (volume deleted or instance terminated)

No servers. No manual work. Just automated cost savings.

---

## 🏗️ Architecture

AWS Lambda (Python 3.12)

│

├── boto3.describe_snapshots()     → Get all owned snapshots

├── boto3.describe_instances()     → Get all running instances

├── boto3.describe_volumes()       → Check if volume still exists

└── boto3.delete_snapshot()        → Delete stale snapshots

│

└── CloudWatch Logs          → Logs every deletion with snapshot ID


---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| AWS Lambda | Serverless function execution |
| Python 3.12 + Boto3 | AWS SDK for automation |
| Amazon EC2 + EBS | Snapshot management |
| AWS IAM | Permissions for Lambda to manage snapshots |
| CloudWatch Logs | Execution logging and monitoring |

---

## 📸 Screenshots

### Lambda Execution — Status: Succeeded
![Lambda Execution](screenshots/Screenshot%204.png)

### CloudWatch Logs — Snapshot Deletion Confirmed
![CloudWatch Logs](screenshots/Screenshot%202.png)

### EBS Snapshots Page — Cleaned Up
![Snapshots Gone](screenshots/Screenshot%205-%20snapshot%20is%20gone.png)

---

## 🚀 How to Deploy

### Prerequisites
- AWS account
- IAM permissions: `DescribeSnapshots`, `DeleteSnapshot`, `DescribeInstances`, `DescribeVolumes`

### Step 1 — Create Lambda Function
- Runtime: Python 3.12
- Paste the code from `cost-optimization.py`
- Set timeout to **10 seconds**

### Step 2 — Attach IAM Policy
- Go to Configuration → Permissions → IAM Role
- Attach `AmazonEC2FullAccess` (or create a custom policy with minimum required permissions)

### Step 3 — Test
- Create a test event with empty JSON `{}`
- Run — check CloudWatch logs for deletion output

---

## 💡 What I Learned

- How AWS Lambda integrates with EC2 and EBS using Boto3
- Writing serverless automation for real cloud cost reduction
- Configuring IAM roles and policies for least-privilege access
- Using CloudWatch Logs to monitor and audit Lambda executions
- Understanding EBS snapshot lifecycle and when snapshots become stale

---

## 🔮 Future Improvements

- [ ] Add SNS notifications when snapshots are deleted
- [ ] Schedule with EventBridge to run automatically every week
- [ ] Add a dry-run mode to preview deletions before executing
- [ ] Implement CloudWatch metrics dashboard for cost savings tracking
- [ ] Extend to clean up unused AMIs and unattached EBS volumes

---

## 👩‍💻 Author

**Sneha Agrawal** — Aspiring Cloud & DevOps Engineer  
🔗 [LinkedIn](https://www.linkedin.com/in/-snehaagrawal/) · [GitHub](https://github.com/sneha020902) · [Portfolio](https://sneha020902.github.io)
