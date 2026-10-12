# T12: Systems Operations

> **Course**: AWS re/Start Curriculum  
> **Module**: T12 – Systems Operations (`M08`)  
> **Total Guides**: 6 Study Guides  
> **Status**: Completed (100%)

---

## Module Overview

The **Systems Operations** module explores the operational lifecycle of workloads deployed to Amazon Web Services. It establishes the foundations of cloud operations engineering: infrastructure provisioning, ongoing health monitoring, configuration security, automated administration via the AWS Command Line Interface (AWS CLI), and structured incident troubleshooting. The module introduces the multi-stage **Café and Bakery** business scenario (Frank, Martha, Sofía, Nikhil) which serves as the overarching practical storyline throughout subsequent modules.

```mermaid
graph TD
    A["T12-0001: Systems Operations Overview"] --> B["T12-0002: SysOps on AWS Core Pillars"]
    A --> C["T12-0003: Troubleshooting Knowledge Base"]
    A --> D["T12-0004: AWS IAM Review & Security"]
    A --> E["T12-0005: AWS CLI (Command Line Interface)"]
    
    B --> F["T12-0006: Café & Bakery Project Scenario & CLI Lab"]
    C --> F
    D --> F
    E --> F

    classDef overview fill:#f9f5d7,stroke:#bdae93,stroke-width:2px;
    classDef foundation fill:#ebdbb2,stroke:#d79921,stroke-width:2px;
    classDef practice fill:#d5c4a1,stroke:#689d6a,stroke-width:2px;

    class A overview;
    class B,C,D,E foundation;
    class F practice;
```

---

## Study Guide Index

| # | Study Guide | Primary Topics Covered |
|---|-------------|------------------------|
| **0001** | [T12-0001-RSSYOP_EN-US_SG_M08_OVERVIEW.md](file:///work/tmp/aws-cf/T12-Systems_Operations/T12-0001-RSSYOP_EN-US_SG_M08_OVERVIEW.md) | SysOps operational mandate, on-prem vs cloud sysadmin evolution, 6 core responsibilities, operational feedback loops |
| **0002** | [T12-0002-RSSYOP_EN-US_SG_M08_SYSOPSAWS.md](file:///work/tmp/aws-cf/T12-Systems_Operations/T12-0002-RSSYOP_EN-US_SG_M08_SYSOPSAWS.md) | Build, Test, Deploy, Monitor, Maintain, Safeguard responsibilities; environment parity (Dev->Test->Prod); automation artifacts |
| **0003** | [T12-0003-SYOP_KNOWLEDGEBASEINTRO_EN.md](file:///work/tmp/aws-cf/T12-Systems_Operations/T12-0003-SYOP_KNOWLEDGEBASEINTRO_EN.md) | Standard 8-pillar Troubleshooting Knowledge Base schema, 7-step cloud troubleshooting methodology, real-world AWS incident runbooks |
| **0004** | [T12-0004-RSSYOP_EN-US_SG_M08_AWSIAM.md](file:///work/tmp/aws-cf/T12-Systems_Operations/T12-0004-RSSYOP_EN-US_SG_M08_AWSIAM.md) | Authentication vs Authorization, IAM Users/Groups/Roles/Policies, SigV4 API signing, credential hygiene, least privilege |
| **0005** | [T12-0005-RSSYOP_EN-US_SG_M08_AWSCLI.md](file:///work/tmp/aws-cf/T12-Systems_Operations/T12-0005-RSSYOP_EN-US_SG_M08_AWSCLI.md) | AWS CLI v2 installation on Linux, `aws configure`, `~/.aws/credentials`, named profiles, JMESPath `--query` filtering, `--dry-run` |
| **0006** | [T12-0006-RSSYOP_EN-US_SG_M08_CAFEINSTALLAWSCLI.md](file:///work/tmp/aws-cf/T12-Systems_Operations/T12-0006-RSSYOP_EN-US_SG_M08_CAFEINSTALLAWSCLI.md) | Café business scenario (Frank, Martha, Sofía, Nikhil), stakeholder management, bastion workstation AWS CLI v2 installation & testing |

---

## The 6 Core SysOps Responsibilities

```mermaid
flowchart LR
    B["1. Build"] --> T["2. Test"]
    T --> D["3. Deploy"]
    D --> M["4. Monitor"]
    M --> MT["5. Maintain"]
    MT --> S["6. Safeguard"]
    S --> B

    classDef phase fill:#ebdbb2,stroke:#d79921,stroke-width:2px;
    class B,T,D,M,MT,S phase;
```

1. **Build**: Provisioning cloud infrastructure resources cleanly and repeatedly using Infrastructure as Code (IaC).
2. **Test**: Verifying environment integrity, connectivity, security policies, and performance before releasing workloads.
3. **Deploy**: Promoting changes across Development, Testing, and Production environments with zero or minimal downtime.
4. **Monitor**: Collecting continuous telemetry via Amazon CloudWatch metrics, logs, and alarms to assess health.
5. **Maintain**: Applying operating system patches, managing backups, optimizing resource usage, and preventing configuration drift.
6. **Safeguard**: Enforcing least-privilege IAM permissions, rotating credentials, encrypting data at rest and in transit, and ensuring audit compliance.
