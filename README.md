# 🛡️ AWS Incident Response Lab

> **End-to-end AWS security incident response simulation** covering CloudWatch alarm triage, unauthorized access investigation with CloudTrail forensics, and S3 public exposure remediation — including a custom Python forensics tool for automated breach analysis.

![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Language](https://img.shields.io/badge/Language-Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Focus](https://img.shields.io/badge/Focus-Incident%20Response-red?style=for-the-badge)

---

## 📌 Project Overview

This project simulates **real-world AWS security incidents** that escalation engineers respond to daily. Three full incident scenarios were built from the ground up, each followed through the complete incident response lifecycle: **detect → investigate → remediate → prevent**.

The project also includes a custom **Python forensics tool** that automatically parses S3 access logs and generates structured incident reports — the same kind of tooling used by Security Operations Centers to triage breaches at scale.

This type of incident handling is performed daily by **AWS Escalation Engineers (ES2), Security Operations teams, and Incident Response analysts** at companies like Amazon, Capital One, and every major cloud-first organization.

---

## 🛠️ Technologies Used

| Service / Tool | Purpose |
|---|---|
| Amazon EC2 | Simulated production server |
| Amazon CloudWatch | Monitoring, alarms, and metric dashboards |
| AWS CloudTrail | API call auditing and forensic investigation |
| AWS IAM | Identity and access management |
| Amazon S3 | Object storage and bucket policies |
| AWS CloudShell | Browser-based AWS CLI |
| Python 3 | Custom forensics tooling |
| S3 Server Access Logging | Forensic data source for breach analysis |

---

## 🚨 Incident Scenarios

### Incident 1 — CloudWatch Alarm Triage (High CPU)

**Scenario:** A CloudWatch alarm fires indicating CPU utilization on `prod-web-server` exceeded the 40% threshold. As the on-call engineer, I investigated, documented the timeline, and resolved the incident.

**Severity:** P2 Performance Incident

**Response Actions:**
- Configured CloudWatch alarm `HighCPU-prod-web-server` with SNS notification
- Triggered controlled CPU spike using stress testing on EC2 instance
- Investigated CPU, network, and disk metrics across the EC2 Monitoring tab
- Documented incident timeline from detection to resolution
- Verified alarm returned to OK state after CPU normalized

**Skills Demonstrated:** Monitoring, alerting, metric analysis, incident documentation

---

### Incident 2 — Unauthorized Access Investigation

**Scenario:** A user account (`temp-contractor`) attempted multiple unauthorized API calls across EC2, IAM, and CloudTrail services. I investigated using CloudTrail forensics, identified the source, and locked the account down.

**Severity:** P1 Security Incident

**Response Actions:**
- Created limited-permission IAM user with only S3 ReadOnly access
- Simulated unauthorized API attempts via AWS CloudShell
- Investigated denied calls in CloudTrail Event History
- Extracted forensic data: source IP, timestamps, error codes, request IDs
- Deactivated the user's access key as immediate containment
- Applied an explicit `EmergencyLockout` deny policy as defense in depth

**Skills Demonstrated:** CloudTrail forensics, IAM security, containment procedures, defense in depth

---

### Incident 3 — S3 Public Exposure Breach (Capital One Pattern)

**Scenario:** An S3 bucket containing sensitive customer data (`customer-database.csv`, `employee-salaries.csv`, `internal-passwords.txt`) was misconfigured to allow public internet access — replicating the exact breach pattern that affected Capital One. I detected the exposure, investigated the impact, and fully remediated the bucket.

**Severity:** P0 Critical Security Incident

**Response Actions:**
- Created S3 bucket with sensitive-looking test data
- Enabled S3 server access logging for forensic capability
- Configured a public bucket policy granting `s3:GetObject` to `Principal: "*"`
- **Demonstrated active exposure** — accessed files via incognito browser with no AWS credentials
- Investigated `PutBucketPolicy` event in CloudTrail to identify when and how the change occurred
- Analyzed S3 access logs to identify all unauthorized access attempts
- Removed public bucket policy
- Re-enabled `Block Public Access` at both bucket and account levels
- Verified remediation by confirming `AccessDenied` response on the previously exposed URL

**Skills Demonstrated:** S3 security, bucket policies, breach forensics, data exposure analysis, account-level preventive controls

---

## 🐍 Custom Python Forensics Tool

A custom Python script (`s3_forensics.py`) was built to automate the analysis of S3 access logs after the breach. The tool:

- Parses raw S3 access logs from any bucket
- Identifies unauthorized GET requests
- Extracts source IPs, timestamps, request IDs, and accessed file names
- Calculates total bytes exfiltrated and number of files exposed
- Outputs a structured JSON incident report ready for SOC handoff

This demonstrates the ability to write **operational tooling** — a key skill for ES2 and Cloud Security Engineering roles where engineers automate repeatable investigation workflows.

---

## 📸 Screenshots

### Incident 1 — CloudWatch Alarm Triage

**Alarm configured in OK state**
![CloudWatch Alarm OK](<img width="2558" height="1212" alt="HighCPU-prod-web-server in OK" src="https://github.com/user-attachments/assets/0b54770b-66fb-42eb-b93e-9d157bfdf569" />


**Alarm in In-Alarm state during CPU spike**
![CloudWatch Alarm Triggered](<img width="2550" height="909" alt="CPU spike graph visible" src="https://github.com/user-attachments/assets/4b5c32d3-72ac-4ab6-8d8f-21265b5e9368" />


**CPU spike visible on metrics graph**
![CloudWatch Metrics Spike](<img width="2544" height="741" alt="metrics graph" src="https://github.com/user-attachments/assets/6f448ce1-7579-474e-a4d4-61040d6b7cda" />

![CloudWatch Metrics Spike](<img width="2559" height="1189" alt="metrics graph1" src="https://github.com/user-attachments/assets/897b604d-73c3-4722-960f-d984b89cdd45" />


**EC2 Monitoring tab showing all metrics during incident**
![EC2 Monitoring](<img width="2311" height="698" alt="EC2 Monitoring tab" src="https://github.com/user-attachments/assets/2f5a57b6-d66d-4a06-933c-d55c3df85b1d" />


**Alarm returned to OK after resolution**
![CloudWatch Alarm Resolved](<img width="2554" height="710" alt="CloudWatch alarm back in OK " src="https://github.com/user-attachments/assets/560618ee-ce62-4994-aafb-c3462300769f" />


---

### Incident 2 — Unauthorized Access Investigation

**Multiple Access Denied errors from temp-contractor in CloudShell**
![CloudShell Access Denied](<img width="701" height="147" alt="CloudShell showing multiple Access Denied" src="https://github.com/user-attachments/assets/2e53f05c-d0b6-45a2-9659-59976b88f65e" />


**CloudTrail event history showing all denied API calls**
![CloudTrail Event History](<img width="867" height="360" alt="CloudTrail event history showing all denied API calls" src="https://github.com/user-attachments/assets/2e128be2-cbe0-47cd-8ab2-a7ed7788ce69" />


**CloudTrail event detail with source IP and error code**
![CloudTrail Event Detail](<img width="2294" height="320" alt="CloudTrail event detail showing sourceIPAddress" src="https://github.com/user-attachments/assets/5f72dc13-5467-439c-8d9a-33018bf16682" />


**Access key deactivated as containment action**
![Access Key Deactivated](<img width="1709" height="467" alt="temp-contractor access key status changed to Inactive" src="https://github.com/user-attachments/assets/4c9df413-92d4-48f2-8fa0-d14a7c43030b" />


**Account locked with EmergencyLockout policy applied**
![Emergency Lockout Applied](<img width="1633" height="607" alt="temp-contractor showing both the deactivated key and EmergencyLockout policy" src="https://github.com/user-attachments/assets/0ea0c768-96f9-4279-b36c-be71835ebc81" />


---

### Incident 3 — S3 Public Exposure Breach

**Bucket created with Block Public Access disabled (vulnerability introduced)**
![S3 Block Public Access Disabled](<img width="1638" height="501" alt="lock Public Access disabled showing the warning" src="https://github.com/user-attachments/assets/7c6280e6-3676-49a1-9b92-13433cab6be8" />


**Sensitive files uploaded to bucket**
![S3 Sensitive Files](<img width="1637" height="557" alt="showing 3 sensitive-looking files uploaded with file sizes and dates" src="https://github.com/user-attachments/assets/e38519c1-77d4-4541-8ed7-0cd3bff18171" />


**Bucket showing "Publicly accessible" badge — the breach is live**
![S3 Public Badge](<img width="1643" height="592" alt="Publicly accessible red badge " src="https://github.com/user-attachments/assets/34efafdc-c7ae-45a5-9425-3cd5fe3c7b44" />


**File accessed publicly via incognito browser with no credentials**
![Public File Access](<img width="963" height="1017" alt="Browser showing the customer-database csv" src="https://github.com/user-attachments/assets/73115e51-2d53-49c3-a0d3-c5bb9207ae18" />

![Public File Access](<img width="604" height="337" alt="Screenshot 2026-04-26 163206" src="https://github.com/user-attachments/assets/8d3364d5-32d6-4049-907c-bbcaaac9f5d8" />


**CloudTrail PutBucketPolicy event showing who made the change**
![CloudTrail PutBucketPolicy](<img width="2294" height="322" alt="CloudTrail event detail showing PutBucketPolicy" src="https://github.com/user-attachments/assets/eb31ce82-ef4a-4c5a-aeb7-1249b2d639b7" />


**S3 access log showing unauthorized GET request**
![S3 Access Logs]()

**Bucket fully remediated — Block Public Access ON**
![S3 Remediated](<img width="1656" height="495" alt="Block all public access" src="https://github.com/user-attachments/assets/752a57af-28c3-423d-bb0e-b26b39744147" />


**AccessDenied confirmed in incognito browser after remediation**
![Access Denied After Remediation](<img width="710" height="285" alt="Incognito browser showing AccessDenied error after remediation" src="https://github.com/user-attachments/assets/e3cd8536-10e3-47c1-8f6b-7cafb33883de" />


**Account-level Block Public Access enabled as preventive control**
![Account Block Public Access](<img width="1682" height="443" alt="S3 account-level Block Public Access" src="https://github.com/user-attachments/assets/bcedea2e-e00d-4c84-80a0-0896d819816b" />


---

## 🔑 Key Concepts Demonstrated

- **Incident Response Lifecycle** — Detection, investigation, containment, eradication, recovery, and lessons learned
- **CloudWatch Monitoring** — Metric alarms, SNS notifications, and dashboard analysis
- **CloudTrail Forensics** — Identifying who did what, when, and from where during a security incident
- **IAM Security** — Least privilege, access key management, and emergency lockout policies
- **S3 Security** — Bucket policies, ACLs, Block Public Access, and access logging
- **Breach Containment** — Immediate access revocation and defense-in-depth controls
- **Preventive Controls** — Account-level configurations that prevent entire classes of incidents
- **Security Tooling** — Writing Python automation to triage incidents at scale

---

## 🎯 Why This Project Matters

These three incident types represent the **most common escalations handled by AWS Escalation Engineers and Cloud Security teams** in production environments today:

- **EC2 performance issues** are the #1 reason customers contact AWS Support
- **Unauthorized API access investigations** are a daily task for Security Operations Centers
- **S3 public exposure incidents** caused breaches at Capital One ($80M fine), Twilio, Verizon, Accenture, and many others

By building, breaking, and resolving each scenario hands-on, this lab demonstrates the **ability to operate confidently in real production AWS environments** under incident pressure.

---

## 🔗 Related Projects

- [LogSentinel](https://github.com/ParmeetGill422/LogSentinel) — Python log analysis and incident report automation tool
- [Active Directory Azure Lab](https://github.com/ParmeetGill422/Active-Directory-Azure-Lab) — Hybrid identity lab with on-prem AD synced to Microsoft Entra ID

---

## 👤 Author

**Parmeet Gill**
- LinkedIn: [linkedin.com/in/parmeet-gill57](https://linkedin.com/in/parmeet-gill57)
- GitHub: [github.com/ParmeetGill422](https://github.com/ParmeetGill422)
- Email: parmeetgill422@gmail.com

---

*Built as part of a cybersecurity portfolio targeting SOC Analyst, IAM Analyst, and Cloud Escalation Engineer roles.*
