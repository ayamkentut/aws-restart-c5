# Module 20: Monitoring and Security

> **Course**: AWS re/Start Curriculum  
> **Module**: 20 — Monitoring and Security  
> **Target Audience**: Cloud Practitioners, Systems Administrators, DevOps Engineers  

---

## 📖 Module Overview

Module 20 focuses on observability, auditing, automated event response, and compliance across AWS infrastructure. It covers real-time metric collection and log ingestion with **Amazon CloudWatch**, deep log analytics using **CloudWatch Logs Insights**, API governance and operational auditing through **AWS CloudTrail**, and serverless SQL forensics over data lake telemetry with **Amazon Athena**.

---

## 📑 Study Guides & Lab Walkthroughs

| Index | Document / Lab | Type | Description |
|---|---|---|---|
| **T20-0001** | [Monitoring and Security Overview](file:///work/tmp/aws-cf/T20-Monitoring_and_Security/T20-0001-RSJAWS_EN-US_SG_M13_MONITORSECURITYOVERVIEW.md) | Study Guide | Foundation of cloud observability, shared responsibility, metric alarms, and audit logs. |
| **T20-0002** | [Amazon CloudWatch](file:///work/tmp/aws-cf/T20-Monitoring_and_Security/T20-0002-RSJAWS_EN-US_SG_M13_AMAZONCLOUDWATCH.md) | Study Guide | Core CloudWatch metrics, alarms, actions, dashboards, and CloudWatch Agent setup. |
| **T20-0003** | [Deep Dive into Amazon CloudWatch](file:///work/tmp/aws-cf/T20-Monitoring_and_Security/T20-0003-RSJAWS_EN-US_SG_M13_DEEPDIVEAMAZONCLOUDWATCH.md) | Study Guide | CloudWatch Logs Insights query syntax, Metric Filters, High-Resolution Metrics, and Cross-Account dashboards. |
| **T20-0004** | [Lab: Monitoring AWS Resources](file:///work/tmp/aws-cf/T20-Monitoring_and_Security/T20-0004-RSJAWS_EN-US_SG_M13_LABOVERVIEWMONITORING.md) | Lab Guide | Hands-on lab configuring CloudWatch alarms, SNS notifications, and testing CPU stress on EC2. |
| **T20-0005** | [AWS CloudTrail](file:///work/tmp/aws-cf/T20-Monitoring_and_Security/T20-0005-RSJAWS_EN-US_SG_M13_AWSCLOUDTRAIL.md) | Study Guide | Governance, compliance, operational auditing, log file integrity validation, and S3 encryption. |
| **T20-0006** | [AWS Service Integration with Amazon Athena](file:///work/tmp/aws-cf/T20-Monitoring_and_Security/T20-0006-RSJAWS_EN-US_SG_M13_AWSSERVICEINTEGRATE.md) | Study Guide | Serverless log analytics over S3, querying CloudTrail and VPC Flow Logs, and cost optimization via Parquet. |

---

## 🗺️ Observability & Auditing Architecture

```mermaid
flowchart TD
    subgraph Sources["Telemetry Sources"]
        EC2["EC2 Instances (CloudWatch Agent)"]
        AWSAPI["AWS API Invocations (Users & Roles)"]
        VPC["Amazon VPC Flow Logs"]
        ALB["Application Load Balancers"]
    end

    subgraph Ingestion["Ingestion & Monitoring"]
        CW["Amazon CloudWatch Metrics & Logs"]
        CT["AWS CloudTrail (Multi-Region Trail)"]
    end

    subgraph Actions["Alerting & Remediation"]
        ALARM["CloudWatch Alarms"]
        SNS["Amazon SNS Topics"]
        AUTO["Auto Scaling / EC2 Actions"]
        EB["Amazon EventBridge Rules"]
        LAMBDA["AWS Lambda Remediations"]
    end

    subgraph Analytics["Audit & Query Storage"]
        S3["Amazon S3 Audit Bucket (WORM / KMS)"]
        ATHENA["Amazon Athena (Presto SQL)"]
        QS["Amazon QuickSight Dashboards"]
    end

    EC2 --> CW
    AWSAPI --> CT
    VPC --> S3
    ALB --> S3

    CW --> ALARM
    ALARM --> SNS
    ALARM --> AUTO
    CW --> EB
    EB --> LAMBDA

    CT --> S3
    CT --> CW

    S3 --> ATHENA
    ATHENA --> QS

    classDef aws fill:#FF9900,stroke:#232F3E,stroke-width:2px,color:#232F3E;
    classDef storage fill:#3F8624,stroke:#232F3E,stroke-width:2px,color:#FFFFFF;
    classDef action fill:#E05243,stroke:#232F3E,stroke-width:2px,color:#FFFFFF;
    class CW,CT aws;
    class S3,ATHENA storage;
    class ALARM,SNS,AUTO,EB,LAMBDA action;
```

---

## 🎯 Key Learning Objectives Summary

- [x] Differentiate between **monitoring** (system behavior and health via CloudWatch) and **auditing** (who did what, when, and from where via CloudTrail).
- [x] Configure standard and high-resolution **CloudWatch Metrics**, alarms with thresholds, and multi-action triggers (SNS, Auto Scaling).
- [x] Deploy and configure the **unified CloudWatch Agent** to collect OS-level memory, swap, and disk metrics.
- [x] Analyze log data using **CloudWatch Logs Insights** queries and create **Metric Filters** to convert pattern matches into numerical alarms.
- [x] Create multi-region **AWS CloudTrails**, understand Management vs Data Events, and enforce **Log File Integrity Validation** using cryptographic digests.
- [x] Execute serverless SQL queries using **Amazon Athena** against CloudTrail, VPC Flow Logs, and ALB access logs partitioned in Amazon S3.
