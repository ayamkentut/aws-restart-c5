# 🥐 The Café and Bakery: Systems Operations Project Scenario & AWS CLI Hands-On Workshop

> **Course**: AWS re/Start  
> **Module**: Systems Operations (M08 – Systems Operations on AWS / Project Scenario)  
> **Source Document**: `T12-Systems_Operations/T12-0006-RSSYOP_EN-US_SG_M08_CAFEINSTALLAWSCLI.pdf`  
> **Target Audience**: AWS re/Start Learners, Cloud Operations Engineers, Junior Systems Administrators, Solutions Architects  

---

## 🎯 Executive Overview & Project Context

In enterprise cloud computing, theoretical knowledge of services and architectural principles must ultimately translate into practical operational execution. Real-world organizations rarely transition to the cloud overnight through a massive "lift-and-shift" migration. Instead, they navigate iterative modernizations driven by tangible business pressures: escalating operational overhead, manual administrative bottlenecks, unpredictable customer demand, and the urgent need to maintain reliability and security within constrained budgets.

This project introduces **The Café and Bakery**, a realistic small-business scenario that serves as the anchoring case study throughout the **AWS Systems Operations (SysOps)** curriculum. By examining the operational friction faced by the café's founders, staff, and advisors, learners explore the vital discipline of **Systems Operations**: bridging business strategy with automated cloud infrastructure.

Furthermore, this guide details the first major technical milestone of the Café's cloud journey: **provisioning an administrative workstation and installing, configuring, and validating the AWS Command Line Interface (AWS CLI v2)** on an Amazon Elastic Compute Cloud (Amazon EC2) Linux instance, establishing foundational AWS Identity and Access Management (IAM) controls, and laying the groundwork for an operational **Troubleshooting Knowledge Base**.

```
┌────────────────────────────────────────────────────────────────────────┐
│              The Café and Bakery: Systems Operations Mandate           │
├────────────────────────────────────────────────────────────────────────┤
│  1. Business Empathy: Bridge technical solutions with human goals      │
│  2. Frugal Architecture: Maximize value while eliminating idle waste   │
│  3. Resilient Engineering: Design fault-tolerant, self-healing systems │
│  4. Operational Automation: Replace manual toil with scriptable APIs   │
│  5. Security by Default: Enforce least-privilege IAM from day one      │
│  6. Continuous Observability: Document, monitor, and runbook failures  │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 📌 Learning Objectives

- [ ] **Analyze the Café and Bakery Business Scenario**: Examine the historical background, operational model, and acute business challenges confronting a growing local enterprise.
- [ ] **Map Stakeholder Personas & Capabilities**: Profile key internal stakeholders (Frank, Martha, Sofía, Nikhil) and external AWS technical advisors (Alejandro, Olivia, Mateo, Faythe) across technical maturity, business roles, and operational priorities.
- [ ] **Address Stakeholder Communication Challenges**: Master the soft skills required to translate technical cloud capabilities (scalability, elasticity, decoupled architectures) into compelling, non-technical business outcomes (cost savings, reduced labor, revenue growth).
- [ ] **Apply AWS Well-Architected Operational Principles**: Formulate technical solutions that embody frugality, high availability, fault tolerance, and baseline security controls.
- [ ] **Deploy & Configure AWS CLI v2 on Linux**: Execute a step-by-step installation of the AWS CLI v2 bundle on an EC2 Linux administrative instance within an Amazon VPC.
- [ ] **Establish Secure Programmatic Authentication**: Configure and differentiate between IAM access keys and EC2 IAM instance profiles, applying least-privilege policies.
- [ ] **Verify Cloud Connectivity & Authentication**: Execute diagnostic verification commands (`aws sts get-caller-identity`, `aws s3 ls`, `aws iam get-user`) to validate configuration integrity.
- [ ] **Contrast CLI Automation with Console Administration**: Articulate why SysOps engineers prioritize scriptable APIs over graphical interfaces while explaining why solution architecture remains interface-agnostic.
- [ ] **Chart the Café's Cloud Transformation Roadmap**: Trace the multi-phase technical roadmap from manual operations to an S3 static web presence, dynamic EC2 application tier, RDS database, and automated Auto Scaling architecture.
- [ ] **Contribute to the Operational Troubleshooting Knowledge Base**: Document structured incident runbooks in the **Security** and **Automation** categories using standardized diagnostic schemas.

---

## 🏪 1. The Café & Bakery Business Scenario & Stakeholder Ecosystem

### Background & Founding Story

In 2016, **Frank** realized his lifelong retirement dream. Following a distinguished career in the United States Navy, Frank and his wife, **Martha**, established a neighborhood bakery and café in their local community. The business was founded on two core passions: Frank's love for artisanal baking—crafting breads and pastries using cherished family recipes handed down by his mother—and a desire to create a welcoming community hub that supplemented their retirement income.

From its inception, the café flourished:
- **Community Anchor**: Frank and Martha actively participate in town gatherings, supplying coffee, artisanal sourdough, and specialty pastries for local fundraisers, farmer's markets, and school functions.
- **Diverse Customer Base**:
  - *Early Morning Commuters*: Seeking fast drip coffee, espresso, and grab-and-go breakfast pastries before catching commuter trains or buses.
  - *Midday Foot Traffic*: Local office workers, remote freelancers, and neighborhood residents seeking casual lunch fare, indoor seating, and WiFi.
  - *Weekend Visitors & Tourists*: Out-of-town travelers visiting regional attractions, seeking high-quality baked goods to take home or gift.
- **Growing Inquiries**: With foot traffic expanding rapidly, travelers frequently ask if the bakery offers online pre-orders, custom celebration cakes, or nationwide shipping for specialty packaged baked goods.

### The Emerging Operational Crisis

While the café's customer popularity has surged, its operational infrastructure remains fundamentally unchanged since 2016. Frank and Martha are experiencing the acute growing pains common to expanding small businesses:

```mermaid
flowchart TD
    subgraph OperationalPains["⚠️ Critical Operational Bottlenecks"]
        P1["📝 Manual Paper Slips<br>Orders written by hand; lost slips cause kitchen chaos"]
        P2["📉 Unpredictable Pastry Demand<br>Baking based on intuition leads to morning stockouts or evening waste"]
        P3["📊 Spreadsheet Overload<br>Martha manually tracks receipts, supplier invoices, and inventory in siloed spreadsheets"]
        P4["⏳ Founder Burnout<br>Frank works 14-hour days baking and managing inventory; unable to step away"]
        P5["🌐 Primitive Digital Presence<br>Static website with no ordering system, real-time inventory, or e-commerce"]
    end
```

1. **Manual Order Tracking & Workflow Bottlenecks**: Daily customer orders are taken on paper tickets and shouted into the kitchen. During the morning peak rush (7:00 AM – 9:00 AM), tickets are frequently misplaced or fulfilled out of order, frustrating high-value regular customers.
2. **Volatile Demand vs. Perishable Inventory**: Baking requires hours of lead time (dough proofing, oven cycling). Without predictive data or online pre-orders, Frank relies on guesswork. Some mornings, popular croissants sell out by 7:45 AM (lost revenue); on rainy mornings, dozens of unsold pastries are discarded at closing (direct cost loss).
3. **Heavy Administrative Overhead**: Martha spends hours every evening manually transcribing cash register receipts, supplier invoices, and payroll into offline spreadsheets. Administrative toil consumes the time she would rather spend interacting with customers or enjoying retirement.
4. **Single Point of Failure (SPOF)**: Frank is the sole custodian of key baking formulas, supplier relationships, and oven calibrations. The entire operational capacity of the café depends entirely on his physical presence.
5. **Impending Personnel Transition**: Their daughter, **Sofía**, who currently serves as café manager and handles inventory purchasing and logistics, is preparing to depart for business university. **Nikhil**, a local secondary school student, works part-time assisting with deliveries and customer service, but lacks formal operational management experience.

---

### Key Stakeholder Personas & Technical Maturity

To architect and deploy an operational cloud solution, a Systems Operations engineer must first analyze the organizational human landscape. In technology consulting, solutions fail when engineers design systems that exceed the users' operational capacity or fail to address their core anxieties.

```mermaid
flowchart TB
    subgraph CafeTeam["🥐 The Café & Bakery Team"]
        Frank["👴 Frank (64)<br>Co-Owner & Head Baker<br><i>Tech Novice (Windows)</i>"]
        Martha["👵 Martha (60)<br>Co-Owner & Accountant<br><i>Spreadsheet Expert (Windows)</i>"]
        Sofia["👩 Sofía (22)<br>Café Manager & Student<br><i>Cloud Enthusiast / Novice Dev</i>"]
        Nikhil["👦 Nikhil (17)<br>Operations Assistant<br><i>Web Designer / Tech Savvy</i>"]
    end

    subgraph AWSTeam["☁️ AWS Advisory & Engineering Team"]
        Alejandro["👨‍💼 Alejandro (25)<br>AWS Tech Account Mgr (TAM)<br><i>Customer Relationship & Strategy</i>"]
        Olivia["👩‍💻 Olivia (31)<br>AWS Solutions Architect (SA)<br><i>Networking, Frugal & Resilient Design</i>"]
        Mateo["👨‍💻 Mateo (29)<br>AWS Senior SysOps Engineer<br><i>Automation, Fault Tolerance, CLI Lead</i>"]
        Faythe["👩‍🔬 Faythe (22)<br>AWS Developer<br><i>Custom Software, Big Data, Security</i>"]
    end

    Sofia <-->|Regular Customer Dialogue| Alejandro
    Alejandro -->|Assembles Advisory Squad| AWSTeam
    Mateo <-->|Hands-on CLI Mentoring| Sofia
    Mateo <-->|IAM Security Setup| Nikhil
    Olivia <-->|Architectural Review| Martha
```

#### Detailed Persona Analysis

| Stakeholder | Role & Background | Tech Profile | Primary Responsibilities | Operational Goals & Anxieties |
| :--- | :--- | :--- | :--- | :--- |
| **Frank** | Co-Owner & Head Baker<br>• Age: 64<br>• Retired US Navy veteran | **Novice**<br>• Windows user<br>• Minimal IT exposure<br>• Low AWS awareness | • Recipe formulation & daily baking<br>• Oven operation & kitchen prep<br>• Customer hospitality & community outreach | • Wants to preserve artisanal baking quality<br>• Worried technology will be confusing or break<br>• Eager to see the café expand globally if feasible |
| **Martha** | Co-Owner & Financial Controller<br>• Age: 60<br>• Retired professional accountant | **Intermediate (Office)**<br>• Windows power user<br>• Spreadsheet expert<br>• Novice cloud understanding | • Bookkeeping, accounting, and tax compliance<br>• Supplier payment processing<br>• Cash register balancing & payroll | • Desires drastic reduction in administrative hours<br>• Demands strict cost predictability (no surprise bills)<br>• Requires clear financial return on investment (ROI) |
| **Sofía** | Café Manager & Future Business Student<br>• Age: 22<br>• Daughter of Frank & Martha | **Enthusiast / Junior**<br>• Completed high school coding<br>• Learning AWS fundamentals<br>• Linux/macOS curious | • Store daily management & opening/closing<br>• Supplier ordering & ingredient logistics<br>• Delivery fulfillment supervision | • Primary cloud champion within the family<br>• Wants to automate workflows before leaving for university<br>• Introduced to AWS by regular customer Alejandro |
| **Nikhil** | Store Operations Assistant & Designer<br>• Age: 17<br>• High school student | **Tech-Savvy Youth**<br>• Basic HTML/CSS web design<br>• Windows/macOS user<br>• Social media native | • Front-of-house customer service<br>• Local bicycle/foot deliveries<br>• Designed the original static HTML café site | • Aspires to take over store management when Sofía leaves<br>• Wants to integrate digital marketing & online ordering<br>• Eager to learn hands-on technical tools (AWS CLI, IAM) |

---

### The AWS Professional Services Team

Recognizing that Sofía needs technical guidance to translate her operational ideas into enterprise-grade cloud architecture, **Alejandro**—an AWS Technical Account Manager who frequents the café for his morning coffee—connects the family with a specialized cross-functional AWS advisory team:

1. **Alejandro (AWS Technical Account Manager - TAM, Age 25)**:
   - *Background*: Dedicated customer advocate within AWS Enterprise Support.
   - *Role*: Acts as the primary relationship liaison. He listens to the customer's high-level business goals, provides strategic architectural guidance, and mobilizes specialized AWS technical resources to help the café expand securely.
2. **Olivia (AWS Solutions Architect - SA, Age 31)**:
   - *Background*: Former senior enterprise network engineer.
   - *Role*: Systems design and infrastructure topology. Olivia specializes in designing **frugal, resilient, and secure** solutions. Her deep networking background ensures the café's VPC, subnets, and routing boundaries follow AWS Well-Architected Framework best practices without overspending.
3. **Mateo (AWS Senior SysOps Engineer, Age 29)**:
   - *Background*: Veteran systems administrator with a strong software development and scripting background.
   - *Role*: Hands-on operational lead. Mateo is an expert in infrastructure automation, continuous integration/continuous deployment (CI/CD), high availability, and disaster recovery (DR). He mentors Sofía and Nikhil directly on command-line operations, IAM security baselines, and operational troubleshooting runbooks.
4. **Faythe (AWS Developer, Age 22)**:
   - *Background*: Software engineer focused on security engineering and big data analytics.
   - *Role*: Application logic and data intelligence. Faythe advises on designing serverless backend APIs, database modeling, and structuring analytical queries to forecast daily pastry demand based on historical point-of-sale data.

---

## 🗣️ 2. Systems Operations Project Objectives & Stakeholder Communication

### Core Project Objectives

The Systems Operations engagement for the Café and Bakery centers on four core objectives:

```mermaid
flowchart LR
    Obj1["<b>1. Communication</b><br>Bridge non-technical business pain points with cloud solutions"] --> Obj2["<b>2. Translation</b><br>Convert customer needs into frugal, resilient, secure designs"]
    Obj2 --> Obj3["<b>3. Execution</b><br>Deploy administrative bastion & AWS CLI automation"]
    Obj3 --> Obj4["<b>4. Governance</b><br>Build institutional Troubleshooting Knowledge Base"]
```

1. **Overcome Stakeholder Communication Barriers**: Identify and overcome the communication hurdles that arise when technologists propose complex digital solutions to traditional, non-technical business owners.
2. **Translate Requirements into Well-Architected Solutions**: Convert loose operational statements (e.g., *"We waste too many blueberry muffins on Tuesdays"*) into concrete AWS architectural components (e.g., event-driven pre-ordering, demand telemetry, automated notifications).
3. **Deliver Technical Implementation**: Provision an administrative environment and establish secure, repeatable programmatic control using the AWS CLI v2 and IAM.
4. **Establish Operational Knowledge Governance**: Institutionalize an operational runbook and **Troubleshooting Knowledge Base** focusing on **Security** and **Automation** to ensure long-term system survivability.

---

### Translating Technical Concepts for Business Stakeholders

A frequent pitfall for SysOps engineers is presenting cloud infrastructure in dense technical jargon. Non-technical business owners like Frank and Martha do not evaluate solutions based on "decoupled microservices" or "CIDR block allocations"; they care about **cost, operational stability, customer satisfaction, and peace of mind**.

To build consensus, the engineering team must translate technical capabilities into clear operational analogies:

| Technical Concept (SysOps Language) | Business Value Translation (Frank & Martha Language) | Operational Impact at the Café |
| :--- | :--- | :--- |
| **Elastic Compute Cloud (EC2) & Auto Scaling** | *"On-demand digital kitchen assistants that arrive during the morning rush and clock out when foot traffic slows."* | The café only pays for server capacity when customers are actively browsing or placing orders online. Zero idle costs overnight. |
| **Amazon S3 Static Web Hosting** | *"A virtually indestructible, ultra-low-cost digital menu board displayed in a display case that never requires electricity or maintenance."* | Nikhil's café website and high-resolution pastry photos are served instantly to customers for pennies per month with 99.999999999% durability. |
| **AWS Identity and Access Management (IAM)** | *"A secure digital key ring where each staff member receives a personalized brass key that only opens their specific cabinet drawer, with no master key left under the mat."* | Sofía, Nikhil, and external advisors have isolated credentials with strict boundaries. Nikhil cannot accidentally delete the billing account or database. |
| **AWS Command Line Interface (AWS CLI)** | *"A master recipe binder containing reusable prep instructions that can bake 500 batches with a single whistle, rather than tapping individual buttons on 500 microwave panels."* | Administrative tasks (backups, instance management, user creation) can be executed in seconds via scripts instead of clicking through hundreds of web pages. |
| **Amazon RDS Multi-AZ Deployment** | *"An identical backup recipe notebook stored in a fireproof safe across town that automatically updates whenever Frank adjusts a measurement in the kitchen."* | If the primary database hardware suffers an outage, the system switches to the standby copy automatically without losing customer orders. |
| **Troubleshooting Knowledge Base (Runbook)** | *"The café's master emergency checklist: step-by-step instructions for what staff must do if an espresso machine leaks or the register loses power."* | Eliminates panic during outages. Any team member can quickly diagnose and resolve recurring system errors without waiting for an outside engineer. |

---

## 💻 3. Technical Hands-On Component: Installing & Verifying AWS CLI in the Café Environment

### The Customer Request

During a morning visit to the café, Mateo asks Sofía about the status of the newly created AWS account:

> **Mateo**: *"Sofía, how is that new AWS account? Do you need any help?"*  
> **Sofía**: *"Hi, Mateo. I have some questions about the AWS CLI and getting that installed. Do you mind helping with the AWS CLI?"*

Sofía explains that while she can navigate the AWS Management Console, she wants to build automated scripts for inventory checks and needs to set up secure, isolated permissions for Nikhil and herself. Mateo agrees to mentor her through provisioning a dedicated administrative workstation on Amazon EC2, installing **AWS CLI v2**, and establishing least-privilege IAM controls.

---

### Architectural Overview

To avoid storing persistent administrative access keys on unmanaged personal laptops, Mateo designs a secure operational bastion architecture. An Amazon EC2 instance running Red Hat Enterprise Linux / Amazon Linux is launched inside a Virtual Private Cloud (VPC). The operator connects via Secure Shell (SSH) and utilizes the AWS CLI to interact securely with AWS APIs.

```mermaid
sequenceDiagram
    autonumber
    actor Operator as SysOps Admin (Sofía / Mateo)
    participant SSH as SSH Client (Terminal)
    participant EC2 as Red Hat / Amazon Linux EC2
    participant CLI as AWS CLI v2 (/usr/local/bin/aws)
    participant STS as AWS STS (get-caller-identity)
    participant IAM as AWS IAM / S3 APIs

    Operator->>SSH: ssh -i cafe-key.pem ec2-user@<Instance-IP>
    SSH->>EC2: Establish encrypted SSH session (Port 22)
    Operator->>EC2: curl & unzip AWS CLI v2 bundle
    Operator->>EC2: sudo ./aws/install
    EC2-->>Operator: Installed: /usr/local/bin/aws (v2.x.x)
    Operator->>CLI: aws configure (Access Key, Secret, Region, Format)
    CLI->>EC2: Writes ~/.aws/credentials & ~/.aws/config
    Operator->>CLI: aws sts get-caller-identity
    CLI->>STS: HTTPS POST + SigV4 Signed Request
    STS-->>CLI: Return JSON (UserId, Account, Arn)
    CLI-->>Operator: Displays authenticated IAM Principal identity
    Operator->>CLI: aws s3 ls
    CLI->>IAM: Evaluate IAM Policy (s3:ListAllMyBuckets)
    IAM-->>CLI: Authorized response (Bucket list)
    CLI-->>Operator: Displays S3 storage environment
```

---

### Step-by-Step Installation: AWS CLI v2 on Linux

The AWS CLI version 2 is the latest major release of the AWS CLI and supports all modern AWS features. Unlike CLI v1 (which required a separate Python runtime and `pip` installation), AWS CLI v2 is distributed as a **self-contained executable package** containing all bundled dependencies.

#### Step 1: Connect to the Administrative EC2 Instance via SSH

Open a local terminal session and connect to the public IP address of the provisioned EC2 Linux instance using your designated private key:

```bash
# Set strict permissions on the SSH private key
chmod 400 cafe-admin-key.pem

# Connect to the EC2 instance (ec2-user is default for Amazon Linux / RHEL)
ssh -i cafe-admin-key.pem ec2-user@54.210.12.85
```

Upon login, verify the operating system details:

```bash
cat /etc/os-release
```

#### Step 2: Install Supporting Utilities

Ensure standard archive and transfer tools are installed:

```bash
# Update package manager repositories
sudo yum update -y

# Verify or install curl and unzip
sudo yum install -y curl unzip
```

#### Step 3: Download the Official AWS CLI v2 Package

Download the official AWS CLI v2 Linux x86_64 installer zip file using `curl`:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

> [!NOTE]
> For ARM-based architectures (such as AWS Graviton instances), replace the download URL with `https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip`.

#### Step 4: Extract the Installer Archive

Unzip the downloaded archive package:

```bash
unzip awscliv2.zip
```

This extracts an installation directory named `aws/` containing the executable installer binary, libraries, and bundled runtime.

#### Step 5: Execute the Installation Script

Run the installation command with superuser privileges:

```bash
sudo ./aws/install
```

By default, the installer places all shared files into `/usr/local/aws-cli` and creates a symbolic link at `/usr/local/bin/aws`.

If updating an existing installation or specifying custom paths:

```bash
# Update or overwrite an existing installation
sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
```

#### Step 6: Validate the Installation

Confirm that the binary is accessible in your system's `PATH` and verify the installed version:

```bash
which aws
aws --version
```

**Expected Sample Output**:
```text
/usr/local/bin/aws
aws-cli/2.15.30 Python/3.11.8 Linux/5.10.210-201.852.amzn2.x86_64 exe/x86_64.amzn.2
```

#### Step 7: Clean Up Temporary Installer Artifacts

To maintain a clean workstation and conserve EBS storage space, remove the downloaded archive and extracted files:

```bash
rm -rf awscliv2.zip ./aws
```

---

### Configuring AWS Credentials & Profiles

To execute commands against AWS APIs, the AWS CLI must locate valid authentication credentials.

```
┌────────────────────────────────────────────────────────────────────────┐
│             AWS CLI Credential Precedence Order (Top to Bottom)        │
├────────────────────────────────────────────────────────────────────────┤
│  1. Command Line Options (--access-key, --secret-key, --profile)       │
│  2. Environment Variables (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY)  │
│  3. AWS IAM Identity Center (SSO token cache)                          │
│  4. Local Configuration Files (~/.aws/credentials & ~/.aws/config)     │
│  5. Amazon ECS Container Task Credentials                              │
│  6. Amazon EC2 Instance Profile Credentials (IAM Role via IMDSv2) ⭐   │
└────────────────────────────────────────────────────────────────────────┘
```

#### Interactive Setup: `aws configure`

For local administration, Mateo assists Sofía in running the interactive configuration wizard:

```bash
aws configure
```

The prompt prompts for four parameters:

```text
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-east-1
Default output format [None]: json
```

#### Understanding the Local Configuration Files

The `aws configure` command generates two hidden files in the user's home directory (`~/.aws/`):

1. **`~/.aws/credentials`** (Stores sensitive secret keys):
   ```ini
   [default]
   aws_access_key_id = AKIAIOSFODNN7EXAMPLE
   aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   ```
2. **`~/.aws/config`** (Stores non-sensitive operational preferences):
   ```ini
   [default]
   region = us-east-1
   output = json
   ```

> [!CAUTION]
> **File Permissions & Security Best Practice**:
> Always restrict access to the `.aws` directory so other users on the operating system cannot read your credentials:
> ```bash
> chmod 700 ~/.aws
> chmod 600 ~/.aws/credentials
> chmod 600 ~/.aws/config
> ```

> [!IMPORTANT]
> **Production SysOps Standard: EC2 Instance Profiles**:
> In enterprise cloud environments, **hardcoding long-term access keys on EC2 instances violates AWS Security Best Practices**. Instead, attach an **IAM Role** directly to the EC2 instance via an **Instance Profile**. The instance automatically retrieves short-term, rotating session credentials through the EC2 Instance Metadata Service (IMDSv2), eliminating the risk of leaked access keys!

---

### Command Verification & Health Checks

Once configured, verify operational connectivity by running fundamental inspection commands:

#### 1. Identity Verification: `aws sts get-caller-identity`

The `sts:GetCallerIdentity` API call does not require specialized IAM permissions and immediately validates who you are:

```bash
aws sts get-caller-identity
```

**Output**:
```json
{
    "UserId": "AIDASOFIAEXAMPLE77",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/sofia-admin"
}
```

*Operational Breakdown*:
- `"UserId"`: The unique internal IAM principal ID.
- `"Account"`: The 12-digit AWS Account ID.
- `"Arn"`: The Amazon Resource Name confirming that the active credentials belong to user `sofia-admin`.

#### 2. Service Connectivity: `aws s3 ls`

Inspect the Amazon S3 storage namespace to confirm programmatic access to storage APIs:

```bash
aws s3 ls
```

**Output**:
```text
2026-03-15 09:12:04 cafe-website-static-assets-2026
2026-03-20 14:45:22 cafe-backup-archives-production
```

#### 3. User Governance Check: `aws iam get-user`

Inspect the specific IAM user configuration:

```bash
aws iam get-user --user-name sofia-admin
```

---

### Architectural Analysis: CLI vs. AWS Management Console

In the checkpoint discussion, Mateo poses two fundamental questions to test Sofía's operational understanding:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        Checkpoint Reflection                           │
├────────────────────────────────────────────────────────────────────────┤
│  Q1: Why would a developer use the AWS CLI instead of the Console?     │
│  Q2: Does the solution architecture change by using the AWS CLI?       │
└────────────────────────────────────────────────────────────────────────┘
```

#### Question 1: Why use the AWS CLI instead of the AWS Management Console?

| Dimension | AWS Management Console | AWS Command Line Interface (AWS CLI) |
| :--- | :--- | :--- |
| **Interaction Mode** | Point-and-click graphical interface (GUI) | Text-based commands and automated shell scripts |
| **Speed & Repetition** | Slow for bulk actions; requires manual page navigation | Instantaneous; loops can iterate across thousands of items |
| **Automation & CI/CD** | Impossible to automate directly without browser testing tools | Seamlessly integrated into bash scripts, cron jobs, and CI/CD pipelines |
| **Human Error Risk** | High; easy to misclick subnets, checkboxes, or security rules | Low; standardized script parameters guarantee consistency |
| **Auditing & Versioning** | Manual notes or screenshots required | Shell scripts and configuration files can be committed to Git |
| **Ideal Use Case** | Ad-hoc resource inspection, initial learning, visual topology | Production deployments, scheduled operations, disaster recovery runbooks |

*Summary*: Developers and SysOps engineers choose the AWS CLI to **automate the creation, provisioning, and maintenance of infrastructure**, expediting administrative tasks by executing hundreds of operations through script files without manual intervention.

#### Question 2: Does the solution architecture change by using the AWS CLI?

**No. The underlying solution architecture does not change in any way.**

Whether an administrator creates an EC2 instance, configures an S3 bucket, or modifies an IAM policy via the AWS Management Console, the AWS CLI, an AWS SDK, or AWS CloudFormation, **every interface ultimately issues an identical, signed HTTP REST API request to the underlying AWS service endpoint**. 

The architecture—consisting of the VPC layout, security boundaries, compute sizing, and database replicas—remains completely identical. Only the **implementation method and operational velocity** change.

```mermaid
flowchart TD
    subgraph ManagementInterfaces["Management Interfaces"]
        Console["🖥️ AWS Management Console<br>(Web Browser)"]
        CLI["💻 AWS CLI v2<br>(Shell Scripts / Terminal)"]
        SDK["⚙️ AWS SDKs / Boto3<br>(Custom Application Code)"]
        IaC["📜 AWS CloudFormation / Terraform<br>(Declarative Code)"]
    end

    subgraph AWSEndpoints["AWS Global API Gateway (REST / HTTPS)"]
        SigV4["🔐 Signature Version 4 (SigV4) Authentication & Authorization"]
    end

    subgraph AWSCloud["AWS Cloud Infrastructure Architecture (Unchanged)"]
        VPC["VPC (10.0.0.0/16)"]
        EC2["Amazon EC2 (Web / App)"]
        S3["Amazon S3 (Assets)"]
        RDS["Amazon RDS (Database)"]
    end

    Console -->|HTTPS API Request| SigV4
    CLI -->|HTTPS API Request| SigV4
    SDK -->|HTTPS API Request| SigV4
    IaC -->|HTTPS API Request| SigV4

    SigV4 --> VPC
    SigV4 --> EC2
    SigV4 --> S3
    SigV4 --> RDS
```

---

## 🗺️ 4. Strategic Cloud Transformation Roadmap for the Café

Working closely with Frank, Martha, Sofía, and Nikhil, the AWS advisory team (Alejandro, Olivia, Mateo, Faythe) outlines a structured, phased transformation roadmap. Rather than attempting a complex, high-risk migration all at once, the café adopts an **iterative cloud adoption model** aligned with the AWS Well-Architected Framework.

```mermaid
flowchart TD
    Phase1["<b>Phase 1: Digital Presence & Static Web</b><br>• Host static HTML/CSS on Amazon S3<br>• Global caching via CloudFront<br>• Custom domain with Route 53<br><i>Investment: Frugal (< $1/month)</i>"]
    
    Phase2["<b>Phase 2: Dynamic App & Order Processing</b><br>• Move ordering backend to Amazon EC2<br>• Secure VPC with public/private subnets<br>• Reverse proxy with Application Load Balancer"]
    
    Phase3["<b>Phase 3: Relational Persistence & State</b><br>• Migrate spreadsheet inventory to Amazon RDS<br>• Multi-AZ automated backups<br>• Strict IAM database authentication"]
    
    Phase4["<b>Phase 4: Elasticity & High Availability</b><br>• Implement EC2 Auto Scaling Groups<br>• Scale compute out/in based on morning rush<br>• Cross-AZ resilient architecture"]
    
    Phase5["<b>Phase 5: Observability & Automated SysOps</b><br>• Amazon CloudWatch metrics & alarms<br>• AWS Systems Manager automated patching<br>• Operational Troubleshooting Knowledge Base"]

    Phase1 --> Phase2
    Phase2 --> Phase3
    Phase3 --> Phase4
    Phase4 --> Phase5
```

### Transformation Phase Breakdown

#### Phase 1: Digital Brand Presence & Static Web Hosting (Immediate)
- **Goal**: Replace Nikhil's unhosted website files with an always-on, globally accessible web showcase for the bakery.
- **AWS Services**: **Amazon S3** (Static Website Hosting), **Amazon CloudFront** (Content Delivery Network), **Amazon Route 53** (DNS), **AWS Certificate Manager** (Free SSL/TLS encryption).
- **Business Outcome**: Customers can view daily menus, bakery history, and store hours from any mobile device. 
- **Operational Profile**: Zero server maintenance, zero patching, virtually zero cost (under $1.00/month under AWS Free Tier).

#### Phase 2: Dynamic Application Tier & Online Ordering (Short-Term)
- **Goal**: Enable customers to place online breakfast and pastry pre-orders for curbside pickup.
- **AWS Services**: **Amazon EC2** (running dynamic web application server), **Application Load Balancer (ALB)**, **Amazon VPC**.
- **Business Outcome**: Morning rush order tickets are captured digitally hours in advance, giving Frank visibility into baking quantities before 5:00 AM.
- **Operational Profile**: Introduction of Linux OS administration, AWS CLI deployment scripts, and security group firewall configurations.

#### Phase 3: Relational Database & State Management (Medium-Term)
- **Goal**: Eliminate Martha's fragile, siloed spreadsheets and establish an auditable, centralized data layer for recipes, sales, and ingredient stock.
- **AWS Services**: **Amazon RDS** (PostgreSQL or MySQL Engine) deployed in a private subnet, with **Multi-AZ** standby replication.
- **Business Outcome**: Real-time inventory tracking. Point-of-sale systems deduct flour, sugar, and butter automatically as pastries are ordered.
- **Operational Profile**: Fully managed automated daily snapshots, automated software minor version patching, and automated multi-AZ hardware failover.

#### Phase 4: Elasticity, Auto Scaling & Fault Tolerance (Long-Term)
- **Goal**: Ensure the ordering system seamlessly handles severe traffic spikes (e.g., local food festival features, holiday pre-order deadlines) without overpaying for idle compute overnight.
- **AWS Services**: **Amazon EC2 Auto Scaling Groups**, **Target Tracking Scaling Policies**, **Amazon SNS** (Order notifications).
- **Business Outcome**: System automatically spins up extra compute instances from 6:30 AM to 9:30 AM, then scales down to a single minimal instance for the rest of the day.
- **Operational Profile**: True cloud elasticity; optimal frugality and high availability.

#### Phase 5: SysOps Observability, Governance & Automation (Continuous)
- **Goal**: Prevent outages through proactive monitoring, institutional runbooks, and automated incident response.
- **AWS Services**: **Amazon CloudWatch** (Alarms, Logs, Dashboards), **AWS Systems Manager** (Patch Manager, Run Command), **AWS CloudTrail** (Security auditing).
- **Business Outcome**: The café operations team resolves technical glitches in minutes using standardized runbooks before customers experience disruption.

---

### Architectural Design Matrix: Frugal, Resilient, and Secure

In accordance with Olivia's design mandate, every architectural tier must satisfy three core pillars:

| Architectural Pillar | Technical Implementation | Direct Value to Frank & Martha |
| :--- | :--- | :--- |
| **Frugal Design** | • S3 static hosting eliminates server idle costs.<br>• EC2 Auto Scaling shuts down surplus compute off-peak.<br>• AWS Budgets alerts Martha when spend exceeds $20/month. | Maximizes profit margins; guarantees no unexpected cloud bills deplete the bakery's cash reserves. |
| **Resilient Design** | • Amazon CloudFront caches content at global edge locations.<br>• Amazon RDS Multi-AZ synchronously replicates database data.<br>• Auto Scaling replaces degraded EC2 instances automatically. | Prevents system downtime during peak sales windows; ensures customer orders are never lost to hardware crashes. |
| **Secure Design** | • IAM least privilege separates administrative and staff roles.<br>• Databases reside strictly in private subnets with no public IPs.<br>• HTTPS enforced end-to-end via AWS Certificate Manager. | Protects sensitive customer payment records and Frank's proprietary family recipes from cyber threats. |

---

## 🛠️ 5. Operational Troubleshooting Knowledge Base: Runbook Entries

As emphasized in the course project instructions, learners must document technical roadblocks encountered during systems operations activities. Below are three standardized entries for the café's **Troubleshooting Knowledge Base**, focusing on **Security** and **Automation**.

---

### Knowledge Base Entry: KB-SEC-001

| Field | Detail |
| :--- | :--- |
| **Entry ID** | `KB-SEC-001` |
| **Category** | **Security & IAM Credentials** |
| **Issue Title** | `InvalidAccessKeyId` / `SignatureDoesNotMatch` Error During AWS CLI Execution |
| **Severity** | High (Complete blockage of administrative operations) |
| **Affected Workload** | Administrative EC2 Linux Workstation / SysOps Management Scripts |

#### 1. Problem Description & Symptoms
When executing any command via the AWS CLI (e.g., `aws s3 ls` or `aws sts get-caller-identity`), the command fails immediately with an authentication error:
```text
An error occurred (InvalidAccessKeyId) when calling the GetCallerIdentity operation: 
The AWS Access Key Id you provided does not exist in our records.
```
Alternatively:
```text
An error occurred (SignatureDoesNotMatch) when calling the ListBuckets operation: 
The request signature we calculated does not match the signature you provided.
```

#### 2. Root Cause Analysis (RCA)
- **Cause A (InvalidAccessKeyId)**: The access key ID entered in `~/.aws/credentials` contains a typo, leading/trailing whitespace, or belongs to an IAM user that has been deleted or deactivated in the AWS Management Console.
- **Cause B (SignatureDoesNotMatch)**: The secret access key contains an omitted character, an invalid copy-paste substitution (such as newline characters), or the local operating system system clock has drifted by more than 15 minutes relative to the AWS time servers (preventing SigV4 cryptographic hash validation).

#### 3. Step-by-Step Remediation Procedure
1. **Verify System Clock Synchronization**:
   ```bash
   # Check current system time against NTP
   timedatectl status
   # Force synchronization if clock drift is detected
   sudo chronyc makestep
   ```
2. **Inspect Configured Credentials**:
   ```bash
   cat ~/.aws/credentials
   ```
   Check for accidental trailing spaces or broken lines.
3. **Reconfigure Active Profile**:
   Run `aws configure` to overwrite with newly generated, validated IAM credentials:
   ```bash
   aws configure
   ```
4. **Best-Practice Remediation (Attach IAM Role)**:
   If running on an EC2 instance, eliminate static keys entirely:
   ```bash
   # Remove static credential files
   rm -f ~/.aws/credentials
   ```
   Attach an IAM Role with appropriate policies (e.g., `CafeAdminRole`) to the EC2 instance via the AWS Console or EC2 Instance Profile API.

#### 4. Preventive Measures & Guardrails
- Enforce IAM access key rotation every 90 days.
- Use EC2 IAM Instance Profiles instead of long-term credentials on virtual machines.
- Configure Amazon CloudWatch alarms on CloudTrail metric filters monitoring `AuthFailure` events.

---

### Knowledge Base Entry: KB-AUT-001

| Field | Detail |
| :--- | :--- |
| **Entry ID** | `KB-AUT-001` |
| **Category** | **Automation & Networking** |
| **Issue Title** | AWS CLI Command Hangs or Times Out on EC2 Instance |
| **Severity** | Critical (Automated scripts stall indefinitely, halting pipelines) |
| **Affected Workload** | Administrative EC2 Workstation in Custom VPC Subnet |

#### 1. Problem Description & Symptoms
When issuing any AWS CLI command (e.g., `aws ec2 describe-instances` or `aws s3 ls`), the command hangs for several minutes with no console output, eventually terminating with:
```text
Connect timeout on endpoint URL: "https://s3.us-east-1.amazonaws.com/"
```
Or:
```text
Could not connect to the endpoint URL: "https://ec2.us-east-1.amazonaws.com/"
```

#### 2. Root Cause Analysis (RCA)
The EC2 instance is located inside a subnet that lacks an outbound route to the public internet, or outbound security controls are blocking HTTPS traffic:
- The instance is in a **private subnet** with no NAT Gateway or VPC Endpoint configured.
- The instance is in a **public subnet**, but the Route Table lacks a default route (`0.0.0.0/0`) pointing to an Internet Gateway (`igw-xxxx`).
- The Security Group attached to the EC2 instance has had its default outbound rule (`All traffic to 0.0.0.0/0`) deleted or restricted, blocking outbound TCP port 443.

#### 3. Step-by-Step Remediation Procedure
1. **Test Network Connectivity to AWS Endpoints**:
   ```bash
   curl -Iv --connect-timeout 5 https://s3.us-east-1.amazonaws.com
   ```
2. **Verify Security Group Outbound Rules**:
   Ensure the EC2 Security Group permits outbound HTTPS (TCP port 443) to `0.0.0.0/0`.
3. **Inspect Subnet Route Table**:
   - If the instance requires direct internet access: Verify the subnet route table has:
     - `Destination: 0.0.0.0/0` $\rightarrow$ `Target: igw-xxxxxxxx`
     - The EC2 instance has an allocated **Public IPv4 address**.
   - If the instance is in a private subnet:
     - Add a route to a **NAT Gateway**: `0.0.0.0/0` $\rightarrow$ `nat-xxxxxxxx`
     - Or configure an **AWS Gateway VPC Endpoint for S3** (`com.amazonaws.us-east-1.s3`) to route AWS CLI storage requests internally through AWS private network paths at zero data transfer cost!

#### 4. Preventive Measures & Guardrails
- Maintain separate, clearly documented route tables for public and private subnets (`rtb-public`, `rtb-private`).
- Implement VPC Endpoints (PrivateLink and Gateway Endpoints) for high-frequency AWS services to eliminate dependence on internet routing.

---

### Knowledge Base Entry: KB-SEC-002

| Field | Detail |
| :--- | :--- |
| **Entry ID** | `KB-SEC-002` |
| **Category** | **Security & IAM Authorization** |
| **Issue Title** | `AccessDenied` / `UnauthorizedOperation` Error During CLI Action |
| **Severity** | Medium (Authentication succeeded, but authorization policy failed) |
| **Affected Workload** | Nikhil's Store Operations CLI Profile |

#### 1. Problem Description & Symptoms
The user authenticates successfully (verified by `aws sts get-caller-identity`), but attempting to list or modify resources produces an access error:
```text
An error occurred (AccessDenied) when calling the ListBuckets operation: 
User: arn:aws:iam::123456789012:user/nikhil-ops is not authorized to perform: 
s3:ListAllMyBuckets because no identity-based policy allows the s3:ListAllMyBuckets action
```

#### 2. Root Cause Analysis (RCA)
- In AWS IAM, all permissions are **implicitly denied by default**.
- Nikhil's IAM user was created without attaching an IAM policy, or the attached policy lacks the required API action (`s3:ListAllMyBuckets`, `s3:GetObject`, etc.).
- An explicit deny exists in a Service Control Policy (SCP) or Permissions Boundary.

#### 3. Step-by-Step Remediation Procedure
1. **Identify the Exact Missing Permission**:
   Read the error message string carefully to pinpoint the required API action (e.g., `s3:ListAllMyBuckets`).
2. **Review Attached User Policies**:
   ```bash
   aws iam list-attached-user-policies --user-name nikhil-ops
   ```
3. **Attach a Least-Privilege Policy**:
   Rather than attaching `AdministratorAccess`, attach a tailored policy granting only necessary permissions for Nikhil's operational duties:
   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "AllowNikhilWebsiteBucketManagement",
               "Effect": "Allow",
               "Action": [
                   "s3:ListBucket",
                   "s3:GetObject",
                   "s3:PutObject"
               ],
               "Resource": [
                   "arn:aws:s3:::cafe-website-static-assets-2026",
                   "arn:aws:s3:::cafe-website-static-assets-2026/*"
               ]
           }
       ]
   }
   ```
4. **Re-test the CLI Command**:
   ```bash
   aws s3 ls s3://cafe-website-static-assets-2026
   ```

#### 4. Preventive Measures & Guardrails
- Enforce group-based permissions (`IAM User Groups`) rather than attaching policies directly to individual users.
- Use the **AWS IAM Policy Simulator** before deploying new policies to production operators.

---

## ❓ 6. Comprehensive Review & Checkpoint Questions

### Checkpoint Question 1: Automation vs. Visual Management
**Question**: *Why would a developer or SysOps administrator use the AWS CLI instead of the AWS Management Console?*  
**Detailed Answer**:  
While the AWS Management Console provides an intuitive visual interface for exploratory work, it is unsuitable for scalable, repeatable systems operations. Engineers utilize the AWS CLI because:
1. **Automation & Scriptability**: CLI commands can be combined into shell scripts, cron jobs, and automation playbooks, executing hundreds of repetitive administration tasks in seconds without human fatigue.
2. **Infrastructure Repeatability**: Commands execute deterministically with identical parameters every time, eliminating configuration drift and human click errors.
3. **CI/CD Integration**: Command-line utilities interface directly with code repositories, build systems (Jenkins, GitHub Actions, AWS CodePipeline), and configuration management tools.
4. **Headless Execution**: The CLI operates across headless virtual machines, bastion hosts, and container runtimes where no web browser or graphical desktop environment exists.

---

### Checkpoint Question 2: Solution Architecture Invariance
**Question**: *Does the underlying solution architecture change by using the AWS CLI instead of the AWS Management Console?*  
**Detailed Answer**:  
**No, the architecture remains completely unchanged.**  
Both the AWS Management Console and the AWS CLI are client interfaces that communicate with the exact same underlying AWS service APIs. When you provision an EC2 instance in the console, the web browser executes an HTTPS call to the EC2 API endpoint (`ec2.amazonaws.com`); when you run `aws ec2 run-instances`, the CLI makes the identical API call signed with Signature Version 4 (SigV4). The resulting cloud components (VPC subnets, routing tables, security groups, EC2 hypervisors, and storage volumes) are identical in every architectural dimension. Only the **mechanism of implementation** differs.

---

### Checkpoint Question 3: Credential Security in Cloud Environments
**Question**: *Why is storing permanent AWS Access Keys (`AKIA...`) on an EC2 instance considered an operational anti-pattern, and what is the SysOps best practice?*  
**Detailed Answer**:  
Storing static access keys in `~/.aws/credentials` on an EC2 instance introduces major security risks:
- If the instance's EBS volume is snapshotted publicly, compromised via an OS vulnerability, or shared inappropriately, the credentials can be extracted and abused indefinitely until manually revoked.
- Keys do not rotate automatically, violating credential hygiene policies.

**SysOps Best Practice**: Attach an **IAM Role** to the EC2 instance via an **Instance Profile**. The instance automatically accesses temporary, short-lived security credentials from the EC2 Instance Metadata Service (IMDSv2). The AWS CLI automatically discovers and uses these temporary credentials without requiring any static keys in `~/.aws/credentials`.

---

### Checkpoint Question 4: Stakeholder Alignment & Frugal Design
**Question**: *Frank and Martha are hesitant to approve cloud initiatives because they fear runaway monthly bills. How should a SysOps engineer structure the architecture and communication to address their financial concerns?*  
**Detailed Answer**:  
1. **Architectural Frugality**:
   - Begin with Phase 1: Host the static website on **Amazon S3** and **Amazon CloudFront**, incurring less than $1.00/month in operational spend.
   - Utilize the **AWS Free Tier** for foundational learning and sandbox validation.
   - Implement scheduled auto-shutdowns on non-production EC2 instances off-hours.
2. **Financial Guardrails**:
   - Establish an **AWS Budget** set to a strict monthly cap (e.g., $25.00/month).
   - Configure **Amazon CloudWatch billing alarms** paired with **Amazon SNS** to alert Martha via email and SMS the moment forecasted spend exceeds 80% of the budget.
3. **Business Communication**:
   - Frame cloud expenditures in terms of direct labor offsets: spending $15/month on automated backups saves Martha 10 hours of manual spreadsheet reconciliation each week.

---

## 📋 7. Operational Summary & Verification Checklist

### Quick Reference: AWS CLI Administrative Commands

| Command | Operational Purpose |
| :--- | :--- |
| `curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"` | Download official AWS CLI v2 bundle |
| `unzip awscliv2.zip && sudo ./aws/install` | Extract and install CLI binaries to `/usr/local/bin/aws` |
| `aws --version` | Validate installed CLI binary, Python runtime, and OS architecture |
| `aws configure` | Interactively set access key, secret key, default region, and output format |
| `aws configure list` | Display active credential sources, profile names, and configuration paths |
| `aws sts get-caller-identity` | Verify active authentication principal (UserId, Account ID, and IAM ARN) |
| `aws s3 ls` | Verify S3 API network reachability and list accessible buckets |
| `aws iam get-user` | Inspect detailed metadata of the authenticated IAM user |
| `timedatectl status` | Check local Linux system clock synchronization (prevents SigV4 drift) |

---

### Post-Implementation Audit Checklist

- [ ] **Workstation Hygiene**: EC2 instance updated with latest security patches (`yum update -y`); temporary installation files (`awscliv2.zip`, `./aws`) deleted.
- [ ] **Binary Verification**: `aws --version` confirms AWS CLI v2 is installed at `/usr/local/bin/aws`.
- [ ] **Permission Hardening**: `.aws/` directory permissions locked down to `700`; credentials file locked to `600`.
- [ ] **Identity Check**: `aws sts get-caller-identity` successfully validates the intended IAM identity with no error.
- [ ] **Least Privilege Enforced**: Dedicated IAM credentials provisioned for Sofía and Nikhil; root account credentials safely stored and locked behind MFA.
- [ ] **Knowledge Base Established**: Initial entries for Security and Automation documented in the team's operational runbook repository.
- [ ] **Transformation Roadmap Aligned**: Stakeholders agree on Phase 1 S3 static hosting deployment prior to building dynamic database tiers.
