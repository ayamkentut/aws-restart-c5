# ☁️ AWS re/Start: Complete Curriculum & Learning Notebook

A repository of learning materials, weekly syllabi, hands-on lab roadmaps, professional soft skills workbooks, and deep-dive technical study guides for the **AWS re/Start** program, converted and structured for personal notebooks (such as **Joplin**, **Obsidian**, or **VS Code**).

---

## 🛡️ Project Standards & AI Guardrails
All directory, file naming, and markdown conventions for this project are codified in:
👉 **[`PROJECT_STANDARDS.md`](./PROJECT_STANDARDS.md)**

Key rules:
- All directory names use the `Txx-` prefix and underscores (`_`) instead of spaces.
- All files use the `Txx-xxxx-[DESCRIPTOR].[ext]` pattern matching their parent directory.
- No Joplin tags (`#joplin` and `> **Tags**:`) in markdown files.
- Audio and video media transcribed with Speech-to-Text and integrated into markdown guides.

---

## 🗂️ Modules & Directory Structure

```
aws-cf/
├── PROJECT_STANDARDS.md                   # AI Guardrails & repository naming standards
├── README.md                              # Repository overview & navigation hub
├── download_all_course_docs.py            # Automated multi-threaded Canvas LTI document downloader
│
├── T01-Agenda/                            # Master 12-Week Curriculum Schedule & Roadmaps
├── T03-Professional_Skills/               # Foundational Soft Skills (FSS) Modules (31 Guides)
├── T05-Cloud_Foundations/                 # AWS Cloud Foundations & Core Services (12 PDFs + 2 Video Demos)
├── T06-Linux/                             # Linux Fundamentals & Administration (14 Guides)
├── T07-Networking/                        # Networking Fundamentals & Architecture (7 PDFs)
├── T08-Security/                          # AWS Security, PKI, IAM & Compliance (18 PDFs + 1 Video Demo)
├── T09-Python_Programming/                # Python Core, Automation & DevOps (10 PDFs)
├── T10-Database/                          # Databases, SQL, RDS & DynamoDB (11 PDFs + 2 Video Demos)
├── T11-AWS_Architecture/                  # Well-Architected Framework & Design (6 PDFs)
├── T12-Systems_Operations/                # SysOps, IAM Review & AWS CLI (6 PDFs)
├── T13-Tooling_and_Automation/            # Systems Manager & Developer Tools (3 PDFs)
├── T14-Servers/                           # S3 Static Hosting & EC2 Server Management (7 PDFs)
├── T15-Scaling_and_Name_Resolution/       # ELB, Auto Scaling, Route 53 & CloudFront (8 PDFs)
├── T16-Serverless_and_Containers/         # Lambda, API Gateway, Step Functions & Containers (7 PDFs)
├── T17-AWS_Databases_Services/            # RDS, Aurora, Redshift & DMS (6 PDFs)
├── T18-AWS_Network_Services/              # VPC Deep Dive, Connectivity & Troubleshooting (5 PDFs)
├── T19-Storage_and_Archiving/             # EBS, EFS, S3, Glacier & Storage Gateway (10 PDFs)
├── T20-Monitoring_and_Security/           # CloudWatch, CloudTrail & Athena Security (6 PDFs)
├── T21-Managing_Resource_Consumption/     # Organizations, Tagging, Billing & Support (6 PDFs + 1 Video Demo)
├── T22-Automated_and_Repeatable_Deployments/ # IaC, CloudFormation & Launch Templates (8 PDFs + 1 Video Demo)
├── T23-AWS_Machine_Learning_and_Generative_AI/ # SkillBuilder & Portfolio Projects (External Web Modules)
└── T24-Exam_Prep/                         # Certification & Post-Graduation (Canvas locked)
```

---

## 🚀 Quick Navigation

### 1. 📅 [T01-Agenda Module](./T01-Agenda/README.md)
- **[T01-0001-reStart_Agenda-4-1.md](./T01-Agenda/T01-0001-reStart_Agenda-4-1.md)**: Master 12-week schedule with day-by-day session breakdowns, activity tags (`📘 Lecture`, `🧪 Lab`, `🏆 Challenge Lab`, `💼 Soft Skills`, `📝 Assessment`), Mermaid curriculum roadmap, and completion checklists.

### 2. 💼 [T03-Professional_Skills Module](./T03-Professional_Skills/README.md)
- **Self-Discovery & Planning**: [T03-0002-Who Am I?](./T03-Professional_Skills/T03-0002-RSTFSS_EN-US_SG_M00_WHOAMI.md), [T03-0003-Skills Inventory](./T03-Professional_Skills/T03-0003-RSTFSS_EN-US_SG_M00_WHOAMIQUESTIONNAIRE.md), [T03-0006-Goal Setting & RACI](./T03-Professional_Skills/T03-0006-RSTFSS_EN-US_SG_M00_GOALSETTINGS.md), [T03-0007-Career Goals & Budgeting](./T03-Professional_Skills/T03-0007-RSTFSS_EN-US_SG_M00_CAREERGOALS.md)
- **Amazon Leadership Principles**: [T03-0009-LP Master Guide (All 16)](./T03-Professional_Skills/T03-0009-RSTFSS_EN-US_SG_M00_LPIntro.md), [T03-0004-Think Big](./T03-Professional_Skills/T03-0004-RSTFSS_EN-US_SG_M00_THINKBIG.md), [T03-0010-Customer Obsession](./T03-Professional_Skills/T03-0010-RSTFSS_EN-US_SG_M00_LPCustomerObsession.md), [T03-0014-Learn and Be Curious](./T03-Professional_Skills/T03-0014-RSTFSS_EN-US_SG_M00_LPLearnAndBeCurious.md), [T03-0019-Earn Trust](./T03-Professional_Skills/T03-0019-RSTFSS_EN-US_SG_M00_LPEarnTrust.md), [T03-0021-Have Backbone](./T03-Professional_Skills/T03-0021-RSTFSS_EN-US_SG_M00_LPHaveBackbone.md)
- **Interviewing & Behavioral STAR**: [T03-0008-Communication & STAR](./T03-Professional_Skills/T03-0008-RSTFSS_EN-US_SG_M00_COMMUNICATIONSTAR.md), [T03-0011-Professional Outlook](./T03-Professional_Skills/T03-0011-RSTFSS_EN-US_SG_M00_OUTLOOK.md), [T03-0013-Emotional Intelligence (EQ)](./T03-Professional_Skills/T03-0013-RSTFSS_EN-US_SG_M00_EMOTIONALINTELLIGENCE.md), [T03-0017-Interview Settings](./T03-Professional_Skills/T03-0017-RSTFSS_EN-US_SG_M00_INTERVIEWSETTINGS.md), [T03-0018-Interview Process](./T03-Professional_Skills/T03-0018-RSTFSS_EN-US_SG_M00_INTERVIEWPROCESS.md), [T03-0020-Hiring Process](./T03-Professional_Skills/T03-0020-RSTFSS_EN-US_SG_M00_HIRINGPROCESS.md), [T03-0029-Elevator Pitch](./T03-Professional_Skills/T03-0029-RSTFSS_EN-US_SG_M00_ELEVATORPITCH.md)
- **Resumes & Digital Branding**: [T03-0015-Digital Presence](./T03-Professional_Skills/T03-0015-RSTFSS_EN-US_SG_M00_DIGITALPRESENCE.md), [T03-0016-Resume Bullets & LinkedIn](./T03-Professional_Skills/T03-0016-RSTFSS_EN-US_SG_M00_DIGITALPRESENCERESUME.md), [T03-0024-Resume Creation Formats](./T03-Professional_Skills/T03-0024-RSTFSS_EN-US_SG_M00_RESUMECREATION.md), [T03-0025-Resume Guidance](./T03-Professional_Skills/T03-0025-RSTFSS_EN-US_SG_M00_RESUMEGUIDANCE.md)
- **Technical Thought Process & Team Collaboration**: [T03-0026-Networking Skills](./T03-Professional_Skills/T03-0026-RSTFSS_EN-US_SG_M00_NETWORKING.md), [T03-0027-Technical Thought Process](./T03-Professional_Skills/T03-0027-RSTFSS_EN-US_SG_M00_TECHTHOUGHTPROCESS.md), [T03-0028-Teamwork & Collaboration](./T03-Professional_Skills/T03-0028-RSTFSS_EN-US_SG_M00_COLLABORATION.md), [T03-0031-Portfolio Project](./T03-Professional_Skills/T03-0031-RSTFSS_EN-US_SG_M00_PORTFOLIOPROJECT.md)
- **Certification & Post-Graduation**: [T03-0012-Ready to Get AWS Certified?](./T03-Professional_Skills/T03-0012-RSMISC_EN-US_SG_M00_AWSCERTIFIED.md), [T03-0023-What's Next (Accenture Case Study)](./T03-Professional_Skills/T03-0023-RSMISC_EN-US_SG_M00_WHATSNEXT.md), [T03-0030-Post-Graduate Resources](./T03-Professional_Skills/T03-0030-RSTFSS_EN-US_SG_M00_POSTGRADRESOURCES.md)

### 3. 🌐 [T05-Cloud_Foundations Module](./T05-Cloud_Foundations/README.md)
- **Cloud Computing Principles**: [T05-0001-Intro to Computing](./T05-Cloud_Foundations/T05-0001-RSCF_SG_EN-US_M01_INTROCOMPUTING.md), [T05-0002-Basic Computing Concepts](./T05-Cloud_Foundations/T05-0002-RSCF_SG_EN-US_M01_BASICCOMPUTINGCONCEPTS.md), [T05-0004-What is Cloud Computing?](./T05-Cloud_Foundations/T05-0004-RSCF_SG_EN-US_M01_WHATISCLOUDCOMPUTING.md), [T05-0005-Advantages of Cloud Computing](./T05-Cloud_Foundations/T05-0005-RSCF_SG_EN-US_M01_ADVANTAGESCLOUDCOMPUTING.md)
- **AWS Infrastructure & Governance**: [T05-0006-What is AWS?](./T05-Cloud_Foundations/T05-0006-FOUN_WHATISAWS_EN.md), [T05-0007-Pricing Fundamentals & TCO](./T05-Cloud_Foundations/T05-0007-RSCF_SG_EN-US_M01_FUNDAMEN-USTALSPRICING.md), [T05-0008-Global Infrastructure](./T05-Cloud_Foundations/T05-0008-RSCF_SG_EN-US_M01_AWSINFRASTRUCTURE.md), [T05-0009-Services & Categories](./T05-Cloud_Foundations/T05-0009-RSCF_SG_EN-US_M01_SERVICESANDCATEGORY.md), [T05-0010-Shared Responsibility Model](./T05-Cloud_Foundations/T05-0010-RSCF_SG_EN-US_M01_SHAREDRESPONSIBILITYMODEL.md)
- **Core Storage (S3)**: [T05-0011-Amazon S3 Foundations](./T05-Cloud_Foundations/T05-0011-RSCF_SG_EN-US_M01_AMAZONS3.md), [T05-0012-Amazon S3 Hands-On Video Guide](./T05-Cloud_Foundations/T05-0012-RSSCHAB_EN-US_Video_M01_S3_Demo.md) *(with full audio transcript)*
- **Core Compute (EC2)**: [T05-0013-Amazon EC2 Foundations](./T05-Cloud_Foundations/T05-0013-RSCF_SG_EN-US_M01_COMPUTEAMAZONEC2.md), [T05-0014-Amazon EC2 Hands-On Video Guide](./T05-Cloud_Foundations/T05-0014-RSSCHAB_EN-US_Video_M01_EC2_Demo.md) *(with full audio transcript)*

### 4. 🐧 [T06-Linux Module](./T06-Linux/README.md)
- **Core Linux OS**: [T06-0001-Introduction to Linux](./T06-Linux/T06-0001-RSLX_EN-US_SG_M02_INTROLINUX.md), [T06-0002-The Linux Command Line](./T06-Linux/T06-0002-RSLX_EN-US_SG_M02_LINUXCMNDLINE.md), [T06-0004-Editing Files (Vim & Nano)](./T06-Linux/T06-0004-RSLX_EN-US_SG_M02_EDITINGFILES.md)
- **Filesystem & Access Control**: [T06-0003-Users and Groups](./T06-Linux/T06-0003-RSLX_EN-US_SG_M02_USERSGROUPS.md), [T06-0005-Working with the File System (FHS)](./T06-Linux/T06-0005-RSLX_EN-US_SG_M02_WORKWITHFILESYSTEM.md), [T06-0006-Working with Files](./T06-Linux/T06-0006-RSLX_EN-US_SG_M02_WORKWITHFILES.md), [T06-0007-Managing File Permissions](./T06-Linux/T06-0007-RSLX_EN-US_SG_M02_MNGFILEPERMS.md)
- **Text Processing & Administration**: [T06-0008-Working Commands & Text Processing](./T06-Linux/T06-0008-RSLX_EN-US_SG_M02_WORKINGCMNDS.md), [T06-0009-Managing Processes & Cron](./T06-Linux/T06-0009-RSLX_EN-US_SG_M02_MNGPROCESSES.md), [T06-0010-Managing Services (systemd)](./T06-Linux/T06-0010-RSLX_EN-US_SG_M02_MNGSERVICES.md)
- **Automation & Operations**: [T06-0011-The Bash Shell](./T06-Linux/T06-0011-RSLX_EN-US_SG_M02_BASHSHELL.md), [T06-0012-Bash Scripts Automation](./T06-Linux/T06-0012-RSLX_EN-US_SG_M02_BASHSCRIPTS.md), [T06-0013-Software Management (yum/apt/dnf)](./T06-Linux/T06-0013-RSLX_EN-US_SG_M02_SOFTWAREMGMT.md), [T06-0014-Managing Log Files & CloudWatch](./T06-Linux/T06-0014-RSLX_EN-US_SG_M02_MNGLOGFILES.md)

### 5. 🌐 [T07-Networking Module](./T07-Networking/README.md)
- **Foundations & Hardware**: [T07-0001-Introduction to Networking](./T07-Networking/T07-0001-RSNF_EN-US_SG_M03_INTRONETWORKING.md), [T07-0002-Networking Concepts & Topologies](./T07-Networking/T07-0002-RSNF_EN-US_SG_M03_NETCONCEPTS.md)
- **Addressing & Subnetting**: [T07-0003-Internet Protocol & Binary](./T07-Networking/T07-0003-RSNF_EN-US_SG_M03_INTERNETPROTOCOL.md), [T07-0005-Introduction to IP Subnetting & CIDR](./T07-Networking/T07-0005-RSNF_EN-US_SG_M03_IPSUBNETTING.md)
- **AWS Cloud Networking**: [T07-0004-Networking in the AWS Cloud (VPC, Subnets, Gateways)](./T07-Networking/T07-0004-RSNF_EN-US_SG_M03_AWSCLOUD.md)
- **Protocols & Emerging Tech**: [T07-0006-Additional Networking Protocols (OSI, TCP/UDP, DNS, CLI)](./T07-Networking/T07-0006-RSNF_EN-US_SG_M03_ADDPROTOCOLS.md), [T07-0007-Wireless, IoT & Mobile Tech](./T07-Networking/T07-0007-RSNF_EN-US_SG_M03_ADDTECHNOLOGIES.md)

### 6. 🔒 [T08-Security Module](./T08-Security/README.md)
- **Foundations & Governance**: [T08-0001-Introduction to Security](./T08-Security/T08-0001-RSSECY_EN-US_SG_M04_SECURITYINTRO.md), [T08-0002-Acceptable Use Policy (AUP)](./T08-Security/T08-0002-RSSECY_EN-US_SG_M04_Acceptable_Use_Policy_Example.md), [T08-0017-AWS Account Creation Best Practices](./T08-Security/T08-0017-RSSECY_EN-US_SG_M04_BESTPRACTICES.md), [T08-0018-AWS Compliance Program & Artifact](./T08-Security/T08-0018-RSSECY_EN-US_SG_M04_AWSCOMPLIANCE.md), [T08-0019-AWS Security Resources & Support Plans](./T08-Security/T08-0019-RSSECY_EN-US_SG_M04_AWSSECURITYRESOURCES.md)
- **Prevention Strategy**: [T08-0003-Security Lifecycle: Prevention](./T08-Security/T08-0003-RSSECY_EN-US_SG_M04_LIFECYCLEPREVENTION.md), [T08-0004-Network Hardening](./T08-Security/T08-0004-RSSECY_EN-US_SG_M04_PREVENTIONNETHARD.md), [T08-0005-Systems Hardening](./T08-Security/T08-0005-RSSECY_EN-US_SG_M04_SYSTEMSHARDENING.md), [T08-0006-Data Security & Encryption](./T08-Security/T08-0006-RSSECY_EN-US_SG_M04_PREVENTIONDATASECURITY.md), [T08-0007-Public Key Infrastructure (PKI)](./T08-Security/T08-0007-RSSECY_EN-US_SG_M04_PREVENTIONPKI.md), [T08-0008-AWS Certificate Manager (ACM) Video Demo](./T08-Security/T08-0008-RSSCHAB_EN-US_Video_M04_ACM_Demo.md) *(with full audio transcript)*
- **Identity & Access Management**: [T08-0009-Identity Management](./T08-Security/T08-0009-RSSECY_EN-US_SG_M04_PREVENTIONIDMGMT.md), [T08-0010-AWS IAM Deep Dive](./T08-Security/T08-0010-RSSECY_EN-US_SG_M04_AWSIAM.md)
- **Detection & Auditing**: [T08-0011-Security Lifecycle: Detection & GuardDuty](./T08-Security/T08-0011-RSSECY_EN-US_SG_M04_DETECTION.md), [T08-0012-AWS CloudTrail](./T08-Security/T08-0012-RSSECY_EN-US_SG_M04_AWSCLOUDTRAIL.md), [T08-0013-AWS Config](./T08-Security/T08-0013-RSSECY_EN-US_SG_M04_AWSCONFIG.md), [T08-0016-AWS Trusted Advisor](./T08-Security/T08-0016-RSSECY_EN-US_SG_M04_AWSTRUSTEDADVISOR.md)
- **Response, Recovery & Analysis**: [T08-0014-Security Lifecycle: Response & Disaster Recovery](./T08-Security/T08-0014-RSSECY_EN-US_SG_M04_RESPONSE.md), [T08-0015-Security Lifecycle: Analysis, RCA & Pen Testing](./T08-Security/T08-0015-RSSECY_EN-US_SG_M04_ANALYSIS.md)

### 7. 🐍 [T09-Python_Programming Module](./T09-Python_Programming/README.md)
- **Core Python Foundations**: [T09-0001-Introduction to Programming](./T09-Python_Programming/T09-0001-RSPP_EN-US_SG_M05_PROGINTRO.md), [T09-0002-Introduction to Python](./T09-Python_Programming/T09-0002-PYTHNPROG_PYTHONINTRO_310_EN.md), [T09-0003-Python Basics](./T09-Python_Programming/T09-0003-RSPP_EN-US_SG_M05_PYTHONBASICS.md), [T09-0004-Flow Control](./T09-Python_Programming/T09-0004-RSPP_EN-US_SG_M05_FLOWCONTROL.md)
- **Modular Design & Tooling**: [T09-0005-Functions](./T09-Python_Programming/T09-0005-RSPP_EN-US_SG_M05_FUNCTIONS.md), [T09-0006-Modules and Libraries](./T09-Python_Programming/T09-0006-RSPP_EN-US_SG_M05_MODSLIBRARIES.md), [T09-0007-Python for System Administration](./T09-Python_Programming/T09-0007-RSPP_EN-US_SG_M05_SYSADMINISTRATION.md), [T09-0008-Debugging and Testing](./T09-Python_Programming/T09-0008-RSPP_EN-US_SG_M05_DEBUGGINGTESTING.md)
- **DevOps & Cloud Automation**: [T09-0009-DevOps and Continuous Integration](./T09-Python_Programming/T09-0009-RSPP_EN-US_SG_M05_DEVOPSCONTINTEGRATION.md), [T09-0010-Configuration Management](./T09-Python_Programming/T09-0010-PYTHNPROG_CONFIGURATIONMGMT_310_EN.md)

### 8. 🗄️ [T10-Database Module](./T10-Database/README.md)
- **Database Foundations & SQL**: [T10-0001-Intro to Databases](./T10-Database/T10-0001-RSDB_EN-US_SG_M06_INTRODATABASES.md), [T10-0002-Database Transactions](./T10-Database/T10-0002-RSDB_EN-US_SG_M06_DBTRANSACTIONS.md), [T10-0003-Tables and Types](./T10-Database/T10-0003-RSDB_EN-US_SG_M06_TABLESTYPES.md), [T10-0004-Inserting Data](./T10-Database/T10-0004-RSDB_EN-US_SG_M06_INSERTINGDATA.md), [T10-0005-Selecting Data](./T10-Database/T10-0005-RSDB_EN-US_SG_M06_SELECTINGDATA.md), [T10-0006-Conditional Search](./T10-Database/T10-0006-RSDB_EN-US_SG_M06_CONDSEARCH.md), [T10-0007-SQL Functions](./T10-Database/T10-0007-RSDB_EN-US_SG_M06_FUNCTIONS.md), [T10-0008-Organizing Data](./T10-Database/T10-0008-RSDB_EN-US_SG_M06_ORGANIZINGDATA.md), [T10-0009-Retrieving Data](./T10-Database/T10-0009-RSDB_EN-US_SG_M06_RETRIEVINGDATA.md)
- **AWS Managed Databases**: [T10-0010-Amazon RDS](./T10-Database/T10-0010-RSDB_EN-US_SG_M06_AWSRDS.md), [T10-0011-Amazon RDS Hands-On Video Guide](./T10-Database/T10-0011-RSSCHAB_EN-US_Video_M06_RDS_Demo.md) *(with full audio transcript)*, [T10-0012-Amazon DynamoDB](./T10-Database/T10-0012-RSDB_EN-US_SG_M06_DYNAMODB.md), [T10-0013-Amazon DynamoDB Hands-On Video Guide](./T10-Database/T10-0013-RSSCHAB_EN-US_Video_M06_DynamoDB_Demo.md) *(with full audio transcript)*

### 9. 🏛️ [T11-AWS_Architecture Module](./T11-AWS_Architecture/README.md)
- **Well-Architected Framework**: [T11-0001-Architecture Overview](./T11-AWS_Architecture/T11-0001-RSJAWS_EN-US_SG_M07_AWSAECHITECTUREOVERVIEW.md), [T11-0002-AWS Well-Architected Framework](./T11-AWS_Architecture/T11-0002-RSJAWS_EN-US_SG_M07_AWSWARCHFRAMEWORK.md), [T11-0003-Operational Excellence & Security](./T11-AWS_Architecture/T11-0003-RSJAWS_EN-US_SG_M07_OPERATIONALEXCELLENCESECURITY.md), [T11-0004-Reliability, Performance & Cost](./T11-AWS_Architecture/T11-0004-RSJAWS_EN-US_SG_M07_RELIABILITYPERFORMANCECOST.md)
- **Reliability & Availability**: [T11-0005-Reliability and High Availability](./T11-AWS_Architecture/T11-0005-RSJAWS_EN-US_SG_M07_RELIABILITYHIGHAVAILABILITY.md), [T11-0006-Café Dynamic Architecture Lab](./T11-AWS_Architecture/T11-0006-RSJAWS_EN-US_SG_M07_CAFELAB_CREATEDYNAMEBSITE.md)

### 10. ⚙️ [T12-Systems_Operations Module](./T12-Systems_Operations/README.md)
- **SysOps & Access Management**: [T12-0001-Systems Operations Overview](./T12-Systems_Operations/T12-0001-RSJAWS_EN-US_SG_M08_SYSOPERATIONSOVERVIEW.md), [T12-0002-IAM Review](./T12-Systems_Operations/T12-0002-RSJAWS_EN-US_SG_M08_IAMREVIEW.md), [T12-0003-Access Keys & Secrets](./T12-Systems_Operations/T12-0003-RSJAWS_EN-US_SG_M08_ACCESSKEYSSECRETS.md)
- **AWS CLI & Automation**: [T12-0004-AWS CLI Deep Dive](./T12-Systems_Operations/T12-0004-RSJAWS_EN-US_SG_M08_AWSCLI.md), [T12-0005-Working with AWS CLI](./T12-Systems_Operations/T12-0005-RSJAWS_EN-US_SG_M08_WORKINGWITHAWSCLI.md), [T12-0006-Troubleshooting AWS CLI](./T12-Systems_Operations/T12-0006-RSJAWS_EN-US_SG_M08_TROUBLESHOOTINGAWSCLI.md)

### 11. 🛠️ [T13-Tooling_and_Automation Module](./T13-Tooling_and_Automation/README.md)
- **Systems Manager & Fleet Management**: [T13-0001-Tooling and Automation Overview](./T13-Tooling_and_Automation/T13-0001-RSJAWS_EN-US_SG_M09_TOOLINGAUTOMATIONOVERVIEW.md), [T13-0002-AWS Systems Manager](./T13-Tooling_and_Automation/T13-0002-RSJAWS_EN-US_SG_M09_AWSSYSTEMSMANAGER.md), [T13-0003-Troubleshooting AWS Systems Manager](./T13-Tooling_and_Automation/T13-0003-RSJAWS_EN-US_SG_M09_TROUBLESHOOTINGAWSSYSMANAGER.md)

### 12. 🖥️ [T14-Servers Module](./T14-Servers/README.md)
- **Static Hosting & Compute**: [T14-0001-Servers Overview](./T14-Servers/T14-0001-RSJAWS_EN-US_SG_M10_SERVERSOVERVIEW.md), [T14-0002-Hosting Static Websites on S3](./T14-Servers/T14-0002-RSJAWS_EN-US_SG_M10_HOSTSTATICWEBSITES3.md), [T14-0003-Amazon EC2 Server Management](./T14-Servers/T14-0003-RSJAWS_EN-US_SG_M10_AMAZONEC2.md), [T14-0004-Troubleshooting Amazon EC2](./T14-Servers/T14-0004-RSJAWS_EN-US_SG_M10_TROUBLESHOOTAMAZONEC2.md)
- **Hands-On Server Labs**: [T14-0005-Hosting Static Website Lab](./T14-Servers/T14-0005-RSJAWS_EN-US_SG_M10_LAB_HOSTSTATICWEBSITE.md), [T14-0006-Deploying a Web Server on EC2 Lab](./T14-Servers/T14-0006-RSJAWS_EN-US_SG_M10_LAB_DEPLOYWEBSERVER.md), [T14-0007-Café Static Website Lab](./T14-Servers/T14-0007-RSJAWS_EN-US_SG_M10_CAFELAB_STATICWEBSITE.md)

### 13. 📈 [T15-Scaling_and_Name_Resolution Module](./T15-Scaling_and_Name_Resolution/README.md)
- **Elastic Load Balancing & Auto Scaling**: [T15-0001-Scaling & Name Resolution Overview](./T15-Scaling_and_Name_Resolution/T15-0001-RSJAWS_EN-US_SG_M11_SCALENAMERESOLUTIONOVERVIEW.md), [T15-0002-Elastic Load Balancing (ALB, NLB, GWLB)](./T15-Scaling_and_Name_Resolution/T15-0002-RSJAWS_EN-US_SG_M11_ELASTICLOADBALANCING.md), [T15-0003-Amazon EC2 Auto Scaling](./T15-Scaling_and_Name_Resolution/T15-0003-RSJAWS_EN-US_SG_M11_AMAZONEC2AUTOSCALING.md), [T15-0004-Troubleshooting Auto Scaling & ELB](./T15-Scaling_and_Name_Resolution/T15-0004-RSJAWS_EN-US_SG_M11_TROUBLESHOOTSCALINGELB.md)
- **DNS, Edge Caching & Scaling Labs**: [T15-0005-Amazon Route 53](./T15-Scaling_and_Name_Resolution/T15-0005-RSJAWS_EN-US_SG_M11_AMAZONROUTE53.md), [T15-0006-Amazon CloudFront CDN](./T15-Scaling_and_Name_Resolution/T15-0006-RSJAWS_EN-US_SG_M11_AMAZONCLOUDFRONT.md), [T15-0007-Scaling & Load Balancing Architecture Lab](./T15-Scaling_and_Name_Resolution/T15-0007-RSJAWS_EN-US_SG_M11_LABOVERVIEWSCALINGELB.md), [T15-0008-Café Highly Available Dynamic App Lab](./T15-Scaling_and_Name_Resolution/T15-0008-RSJAWS_EN-US_SG_M11_CAFELAB_HIGHAVAILDYNAMICAPP.md)

### 14. ⚡ [T16-Serverless_and_Containers Module](./T16-Serverless_and_Containers/README.md)
- **Serverless & Event-Driven**: [T16-0001-Serverless & Containers Overview](./T16-Serverless_and_Containers/T16-0001-RSJAWS_EN-US_SG_M12_SERVERLESSCONTAINEROVERVIEW.md), [T16-0002-Amazon EventBridge](./T16-Serverless_and_Containers/T16-0002-RSJAWS_EN-US_SG_M12_AMAZONEVENTBRIDGE.md), [T16-0003-AWS Lambda Deep Dive](./T16-Serverless_and_Containers/T16-0003-RSJAWS_EN-US_SG_M12_AWSLAMBDA.md), [T16-0004-Amazon API Gateway](./T16-Serverless_and_Containers/T16-0004-RSJAWS_EN-US_SG_M12_AMAZONAPIGATEWAY.md), [T16-0005-AWS Step Functions](./T16-Serverless_and_Containers/T16-0005-RSJAWS_EN-US_SG_M12_AWSSTEPFUNCTIONS.md)
- **Containers & Serverless Labs**: [T16-0006-Containers on AWS (ECS, EKS, Fargate)](./T16-Serverless_and_Containers/T16-0006-RSJAWS_EN-US_SG_M12_CONTAINERSAWS.md), [T16-0007-Serverless Architecture Lab](./T16-Serverless_and_Containers/T16-0007-RSJAWS_EN-US_SG_M12_LABOVERVIEWSERVERLESS.md)

### 15. 🗄️ [T17-AWS_Databases_Services Module](./T17-AWS_Databases_Services/README.md)
- **Managed Relational & NoSQL**: [T17-0001-AWS Database Services Overview](./T17-AWS_Databases_Services/T17-0001-RSJAWS_EN-US_SG_M12_AWSDATABASESERVICESOVERVIEW.md), [T17-0002-Amazon RDS & Multi-AZ Aurora](./T17-AWS_Databases_Services/T17-0002-RSJAWS_EN-US_SG_M12_AMAZONRDS.md), [T17-0003-Amazon DynamoDB Deep Dive](./T17-AWS_Databases_Services/T17-0003-RSJAWS_EN-US_SG_M12_AMAZONDYNAMODB.md)
- **Analytics, Migration & Database Labs**: [T17-0004-Amazon Redshift](./T17-AWS_Databases_Services/T17-0004-RSJAWS_EN-US_SG_M12_AMAZONREDSHIFT.md), [T17-0005-AWS Database Migration Service (DMS)](./T17-AWS_Databases_Services/T17-0005-RSJAWS_EN-US_SG_M12_AWSDATABASEMIGRATIONSVC.md), [T17-0006-Café Database Migration Lab](./T17-AWS_Databases_Services/T17-0006-RSJAWS_EN-US_SG_M12_CAFELAB_MIGRATEDATABASE.md)

### 16. 🌐 [T18-AWS_Network_Services Module](./T18-AWS_Network_Services/README.md)
- **VPC Deep Dive & Routing**: [T18-0001-Network Services Overview](./T18-AWS_Network_Services/T18-0001-RSJAWS_EN-US_SG_M12_NETWORKSERVICESOVERVIEW.md), [T18-0002-Amazon VPC Architecture](./T18-AWS_Network_Services/T18-0002-RSJAWS_EN-US_SG_M12_AMAZONVPC.md), [T18-0003-VPC Peering, Endpoints & Gateways](./T18-AWS_Network_Services/T18-0003-RSJAWS_EN-US_SG_M12_VPCPEERING.md)
- **Troubleshooting & Network Labs**: [T18-0004-Troubleshooting VPC Networking](./T18-AWS_Network_Services/T18-0004-RSJAWS_EN-US_SG_M12_TROUBLESHOOTINGAMAZONVPC.md), [T18-0005-VPC Networking Lab](./T18-AWS_Network_Services/T18-0005-RSJAWS_EN-US_SG_M12_LABOVERVIEWVPC.md)

### 17. 💾 [T19-Storage_and_Archiving Module](./T19-Storage_and_Archiving/README.md)
- **Block & File Storage**: [T19-0001-Storage & Archiving Overview](./T19-Storage_and_Archiving/T19-0001-RSJAWS_EN-US_SG_M13_STORAGEANDARCHIVINGOVERVIEW.md), [T19-0002-Amazon EBS](./T19-Storage_and_Archiving/T19-0002-RSJAWS_EN-US_SG_M13_AMAZONEBS.md), [T19-0003-Amazon EFS](./T19-Storage_and_Archiving/T19-0003-RSJAWS_EN-US_SG_M13_AMAZONEFS.md), [T19-0004-Troubleshooting EBS & EFS](./T19-Storage_and_Archiving/T19-0004-RSJAWS_EN-US_SG_M13_TROUBLESHOOTEBSEFS.md)
- **Object & Hybrid Storage**: [T19-0005-Amazon S3 & Glacier Deep Dive](./T19-Storage_and_Archiving/T19-0005-RSJAWS_EN-US_SG_M13_AMAZONS3.md), [T19-0006-AWS Storage Gateway](./T19-Storage_and_Archiving/T19-0006-RSJAWS_EN-US_SG_M13_AWSSTORAGEGATEWAY.md), [T19-0007-Troubleshooting S3](./T19-Storage_and_Archiving/T19-0007-RSJAWS_EN-US_SG_M13_TROUBLESHOOTS3.md)
- **Hands-On Storage Labs**: [T19-0008-EBS & EFS Storage Lab](./T19-Storage_and_Archiving/T19-0008-RSJAWS_EN-US_SG_M13_LABOVERVIEWEBSANDEFS.md), [T19-0009-S3 & Glacier Storage Lab](./T19-Storage_and_Archiving/T19-0009-RSJAWS_EN-US_SG_M13_LABOVERVIEWS3GLACIER.md), [T19-0010-Café S3 Static Hosting Lab](./T19-Storage_and_Archiving/T19-0010-RSJAWS_EN-US_SG_M13_CAFELAB_REARCHSTATICWEBSITE.md)

### 18. 🔍 [T20-Monitoring_and_Security Module](./T20-Monitoring_and_Security/README.md)
- **Observability & Alarms**: [T20-0001-Monitoring & Security Overview](./T20-Monitoring_and_Security/T20-0001-RSJAWS_EN-US_SG_M13_MONITORSECURITYOVERVIEW.md), [T20-0002-Amazon CloudWatch](./T20-Monitoring_and_Security/T20-0002-RSJAWS_EN-US_SG_M13_AMAZONCLOUDWATCH.md), [T20-0003-Deep Dive into CloudWatch](./T20-Monitoring_and_Security/T20-0003-RSJAWS_EN-US_SG_M13_DEEPDIVEAMAZONCLOUDWATCH.md), [T20-0004-Monitoring Lab Guide](./T20-Monitoring_and_Security/T20-0004-RSJAWS_EN-US_SG_M13_LABOVERVIEWMONITORING.md)
- **Auditing & Forensics**: [T20-0005-AWS CloudTrail](./T20-Monitoring_and_Security/T20-0005-RSJAWS_EN-US_SG_M13_AWSCLOUDTRAIL.md), [T20-0006-Athena Log Analytics Integration](./T20-Monitoring_and_Security/T20-0006-RSJAWS_EN-US_SG_M13_AWSSERVICEINTEGRATE.md)

### 19. 💰 [T21-Managing_Resource_Consumption Module](./T21-Managing_Resource_Consumption/README.md)
- **FinOps & Governance**: [T21-0001-Resource Consumption Overview](./T21-Managing_Resource_Consumption/T21-0001-RSJAWS_EN-US_SG_M13_MANAGERESOURCECONSUMPTION.md), [T21-0002-AWS Organizations & SCPs](./T21-Managing_Resource_Consumption/T21-0002-RSJAWS_EN-US_SG_M13_AWSORGANIZATIONS.md), [T21-0003-Tagging Strategies & Governance](./T21-Managing_Resource_Consumption/T21-0003-RSJAWS_EN-US_SG_M13_TAGGING.md)
- **Cost Analytics & Support**: [T21-0004-Billing Dashboard Video Demo](./T21-Managing_Resource_Consumption/T21-0004-RSSCHAB_EN-US_Video_M14_Billing_Dashboard_Demo.md) *(with full audio transcript)*, [T21-0005-Cost Management Best Practices](./T21-Managing_Resource_Consumption/T21-0005-RSJAWS_EN-US_SG_COSTMGMTBESTPRACTICES.md), [T21-0006-AWS Support Services & Trusted Advisor](./T21-Managing_Resource_Consumption/T21-0006-RSJAWS_EN-US_SG_M13_AWSSUPPORTSVCS.md), [T21-0007-Café Resource Optimization Lab](./T21-Managing_Resource_Consumption/T21-0007-RSJAWS_EN-US_SG_M13_CAFEOPTIMIZERESOURCEUTILIZATION.md)

### 20. 🚀 [T22-Automated_and_Repeatable_Deployments Module](./T22-Automated_and_Repeatable_Deployments/README.md)
- **Configuration & Machine Images**: [T22-0001-Deployments Overview](./T22-Automated_and_Repeatable_Deployments/T22-0001-RSJAWS_EN-US_SG_M13_AUTOMATEREPEATDEPLOYOVERVIEW.md), [T22-0002-Configuration Management in the Cloud](./T22-Automated_and_Repeatable_Deployments/T22-0002-JAWS_AUTOMATEREPEATDEPLOYMENTS_310_EN.md), [T22-0003-AMI Building Strategy](./T22-Automated_and_Repeatable_Deployments/T22-0003-RSJAWS_EN-US_SG_M13_AMIBUILDINGSTRATEGY.md)
- **Launch Templates & IaC**: [T22-0004-Amazon EC2 Launch Templates](./T22-Automated_and_Repeatable_Deployments/T22-0004-RSJAWS_EN-US_SG_M13_AMAZONEC2LAUNCHTEMP.md), [T22-0005-Launch Template Video Demo](./T22-Automated_and_Repeatable_Deployments/T22-0005-RSSCHAB_EN-US_Video_M14_EC2_Launch_Template_Demo.md) *(with full audio transcript)*, [T22-0006-Infrastructure as Code (IaC)](./T22-Automated_and_Repeatable_Deployments/T22-0006-RSJAWS_EN-US_SG_M13_INFRASTRUCTUREASCODE.md), [T22-0007-JSON and YAML Formats](./T22-Automated_and_Repeatable_Deployments/T22-0007-RSJAWS_EN-US_SG_M13_JSONYAML.md)
- **CloudFormation & Troubleshooting**: [T22-0008-AWS CloudFormation Deep Dive](./T22-Automated_and_Repeatable_Deployments/T22-0008-RSJAWS_EN-US_SG_AWSCLOUDFORMATION.md), [T22-0009-Troubleshooting CloudFormation Stacks](./T22-Automated_and_Repeatable_Deployments/T22-0009-RSJAWS_EN-US_SG_M13_TROUBLESHOOTAWSCLOUDFORMATION.md)


