# 🛡️ Smart Vault: Automated EC2 Backup Solution on AWS

This project implements a fully automated "Smart Vault" backup solution for EC2 instances in AWS, using EventBridge, Lambda, EBS snapshots, and supporting services like SNS, S3, and CloudWatch.

## 📌 Objective

Automatically create and manage EBS snapshots for EC2 instances that are tagged for backup, while ensuring storage efficiency, monitoring, and fault alerts.

---

## 📐 Architecture Overview

![Architecture Diagram](./assets/smart_vault_architecture.png)

### 🔧 Core Components

- **EC2 Instances**: Production instances to be backed up. Tag `backup=True` to include them in the process.
- **EBS Volumes**: Attached storage to EC2 instances that will be snapshotted.
- **Amazon EventBridge**: Triggers Lambda function on a scheduled basis (e.g., every 24 hours).
- **AWS Lambda**: Main backup handler function.
  - Finds EC2s with `backup=True` tag
  - Identifies and snapshots their EBS volumes
  - Deletes older snapshots to enforce retention policy
- **Amazon SNS**: Sends alerts in case of failures or snapshot issues.
- **Amazon CloudWatch**: Tracks metrics like total snapshots, snapshot storage size, and errors.
- **Amazon S3** *(Optional)*: Stores metadata logs or snapshot audit data.

---

## 🧠 Features

- Tag-based EC2 discovery for backups
- Scheduled, serverless snapshot creation
- Snapshot retention logic (delete older snapshots)
- Failure alerts via SNS
- CloudWatch metrics and logs
- Optional S3 storage for audit metadata

---

## 🏗️ How It Works

1. **Tagging**: Mark EC2 instances with `backup=True`.
2. **Scheduling**: EventBridge triggers Lambda daily or at your chosen interval.
3. **Snapshot Management**:
   - Lambda fetches EC2 instances with the tag
   - Loops over attached volumes and creates snapshots
   - Applies metadata (instance ID, timestamp)
   - Deletes snapshots older than N days
4. **Monitoring and Alerting**:
   - CloudWatch logs all operations
   - SNS notifies on any failure events

---

## 🔐 Security & IAM

Ensure proper IAM roles for:
- EC2 (to allow snapshot operations)
- Lambda (to access EC2, EBS, SNS, CloudWatch, and optionally S3)

Optional: Use AWS KMS to encrypt snapshots.

---

## 🧪 Testing

- Tag EC2 instances and verify snapshot creation in console
- Check CloudWatch Logs for function output
- Induce failure (e.g., revoke permissions) to test SNS alerts

---

## 📁 File Structure

```
/smart-vault/
│
├── lambda/
│   └── snapshot_handler.py     # Lambda function code
├── assets/
│   └── smart_vault_architecture.png  # Architecture diagram
├── README.md
```

---

## 📬 Contact

For questions or contributions, open an issue or pull request.

---

© 2025 – Smart Vault Backup Engine