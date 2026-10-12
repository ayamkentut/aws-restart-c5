# 🛡️ AWS Identity and Access Management (IAM): Architecture, Security Credentials, & Policy Evaluation Engine

> **Course**: AWS re/Start  
> **Module**: Security (M04 – AWS Identity and Access Management)  
> **Source Document**: `T08-Security/T08-0010-RSSECY_EN-US_SG_M04_AWSIAM.pdf`  
> **Target Audience**: AWS re/Start Learners, Cloud Practitioners, Solutions Architects, and Security Engineers  

---

## 🎯 Module Overview

**AWS Identity and Access Management (IAM)** serves as the central control plane and foundational security service for Amazon Web Services. In traditional data centers, access control is enforced through physical gates, perimeter firewalls, and on-premises domain controllers. In cloud computing, resources are software-defined and distributed globally across diverse infrastructure components. Identity becomes the ultimate security perimeter.

IAM enables cloud administrators to securely control access to AWS services and resources. It provides the mechanisms to answer two essential questions for every incoming API request:
1. **Authentication (AuthN)**: *Who is making the request?* (Verifying the claimed identity of a human user or programmatic entity).
2. **Authorization (AuthZ)**: *What actions is the identity allowed to perform on which resources, and under what conditions?* (Evaluating policies to determine permissions).

IAM is a **global service** natively integrated across the entire AWS ecosystem. It operates with high availability without requiring per-Region deployment or administrative overhead, and it is provided as a core capability of an AWS account at **no additional charge**.

```mermaid
flowchart TD
    subgraph RequestSource["Incoming Request / Principal"]
        Console["🖥️ AWS Management Console<br>(Human User)"]
        CLI["💻 AWS CLI / SDKs / Scripts<br>(Developer / Admin)"]
        Workload["⚙️ AWS Service / Workload<br>(EC2, Lambda, Container)"]
    end

    subgraph IAMService["🛡️ AWS Identity and Access Management (Global)"]
        AuthN["1. Authentication (AuthN)<br>• Username & Password + MFA<br>• Access Keys (Key ID + Secret Key)<br>• Temporary STS Credentials"]
        
        EvalEngine{"2. Policy Evaluation Engine<br>(Authorization / AuthZ)"}
        
        subgraph Policies["JSON Policies"]
            IdPol["Identity-Based Policies<br>(Users, Groups, Roles)"]
            ResPol["Resource-Based Policies<br>(S3, KMS, SQS)"]
        end
    end

    subgraph AWSResources["☁️ Target AWS Resources"]
        EC2["Amazon EC2"]
        S3["Amazon S3"]
        DynamoDB["Amazon DynamoDB"]
        RDS["Amazon RDS"]
    end

    Console --> AuthN
    CLI --> AuthN
    Workload --> AuthN

    AuthN -->|Authenticated Identity| EvalEngine
    IdPol -.-> EvalEngine
    ResPol -.-> EvalEngine

    EvalEngine -->|Explicit Allow & No Deny| AWSResources
    EvalEngine -->|Explicit Deny OR Implicit Deny| Block["🚫 HTTP 403 Access Denied"]
```

---

## 📌 Learning Objectives

By completing this module and thoroughly reviewing these study materials, you will be able to:

- [ ] **Define AWS IAM & Core Characteristics**: Explain the operational role of AWS IAM, its global operational scope, zero-cost pricing model, and integration across AWS services.
- [ ] **Distinguish Authentication from Authorization**: Differentiate how IAM identifies callers (AuthN) versus how IAM evaluates and enforces permissions (AuthZ).
- [ ] **Classify IAM Security Credentials**: Identify and compare the six primary types of security credentials (root credentials, IAM console passwords, access key pairs, SSH keys, X.509 certificates, and AWS STS temporary credentials).
- [ ] **Hardening Identities with MFA**: Implement Multi-Factor Authentication (MFA) across hardware, virtual TOTP apps, and evaluate the operational role of MFA in securing privileged identities.
- [ ] **Compare IAM Entities**: Contrast the security posture, operational lifecycle, and best-use patterns of the **AWS Account Root User**, **IAM Users**, **IAM User Groups**, and **IAM Roles**.
- [ ] **Deconstruct IAM Roles & Temporary Credentials**: Analyze how IAM roles delegate temporary access using AWS Security Token Service (AWS STS), distinguishing between **Trust Policies** and **Permissions Policies**.
- [ ] **Parse JSON Policy Structure**: Dissect standard IAM policy syntax, explaining the semantic function of `Version`, `Statement`, `Sid`, `Effect`, `Principal`, `Action`/`NotAction`, `Resource`/`NotResource`, and `Condition`.
- [ ] **Categorize IAM Policy Types**: Differentiate between **Identity-based Policies** (AWS Managed, Customer Managed, and Inline) and **Resource-based Policies**.
- [ ] **Master the IAM Policy Evaluation Engine**: Trace the exact deterministic evaluation workflow, proving that **Explicit Deny overrides all Allows** and applying the concept of **Default / Implicit Deny**.
- [ ] **Apply AWS IAM Best Practices**: Implement the AWS Well-Architected Framework recommendations for root credential safeguarding, individual identities, group permissions, least privilege, credential rotation, and role delegation.

---

## 1. AWS Identity and Access Management (IAM) Fundamentals

### 1.1 What is AWS IAM?

> **Definition**: **AWS Identity and Access Management (IAM)** is a web service that helps administrators securely control authentication and authorization for users, applications, and AWS resources within their AWS environment.

IAM allows organizations to centrally administer security controls:
- **Centralized Security Management**: Create and manage identities, passwords, access keys, and permission boundaries from a single administrative interface.
- **Granular Permissions**: Specify exact actions that specific users or services can execute on designated resources (e.g., allow user `Li` to inspect an Amazon S3 bucket, but deny permissions to delete objects or stop EC2 instances).
- **Zero Cost**: IAM is an integrated core capability of every AWS account. There are no additional subscription fees, per-user charges, or operational costs associated with creating users, groups, roles, or policies.
- **Global Scope**: IAM does not run in a specific AWS Region. Policies and identities created in IAM are automatically distributed globally to all AWS Regions worldwide.

```mermaid
flowchart LR
    subgraph GlobalPlane["🌐 IAM Global Control Plane"]
        IAM["AWS IAM<br>(No Region Selection Needed)"]
    end

    subgraph Regions["Enforced Consistently Across All Regions"]
        US_EAST["us-east-1<br>(N. Virginia)"]
        EU_WEST["eu-west-1<br>(Ireland)"]
        AP_SE["ap-southeast-1<br>(Singapore)"]
    end

    IAM ==> US_EAST
    IAM ==> EU_WEST
    IAM ==> AP_SE
```

### 1.2 Authentication (AuthN) vs. Authorization (AuthZ)

Understanding the distinction between authentication and authorization is vital for security engineering and AWS certification:

```mermaid
flowchart LR
    Caller["User or Program"] -->|1. Credentials Presented| AuthN["🔑 Authentication (AuthN)<br>'Who are you?'"]
    AuthN -->|Identity Confirmed| AuthZ["📜 Authorization (AuthZ)<br>'What are you allowed to do?'"]
    AuthZ -->|Evaluates Policies| Decision{"Decision"}
    Decision -->|Allowed| Action["✅ Execute API Action"]
    Decision -->|Denied| Deny["🚫 Access Denied"]
```

| Dimension | Authentication (AuthN) | Authorization (AuthZ) |
| :--- | :--- | :--- |
| **Fundamental Question** | *"Who are you?"* | *"What are you permitted to do?"* |
| **Verification Mechanism** | Validates identity claims using known credentials (passwords, access keys, MFA tokens, certificates). | Evaluates formal permission statements (JSON policies) against the request context. |
| **Execution Sequence** | **First Step**: A caller must prove identity before authorization begins. | **Second Step**: Occurs only after authentication succeeds. |
| **IAM Implementation** | IAM User credentials, console sign-in URLs, STS token validation. | IAM Policies (Identity-based, Resource-based, SCPs, Permission Boundaries). |
| **Outcome** | Confirmed identity principal (e.g., `arn:aws:iam::123456789012:user/Jane`). | Deterministic authorization decision: **Allow** or **Deny**. |

> [!NOTE]
> IAM eliminates the hazardous anti-pattern of sharing master passwords, shared accounts, or private API keys across team members. By providing granular authentication and authorization, every engineer and application process receives a discrete, auditable identity.

---

## 2. AWS Security Credentials Taxonomy

AWS provides multiple distinct security credential types. Each credential serves a dedicated communication channel, security domain, and operational use case.

```mermaid
flowchart TD
    Creds["🔐 AWS Security Credentials"]
    
    Human["👤 Human Access Credentials"]
    Prog["🤖 Programmatic / Service Credentials"]
    Temp["⏳ Ephemeral / Temporary Credentials"]

    Creds --> Human
    Creds --> Prog
    Creds --> Temp

    Human --> RootCred["Root Email & Password<br>(Break-glass only)"]
    Human --> UserPass["IAM Username & Password<br>(AWS Console)"]
    Human --> MFA["Multi-Factor Authentication<br>(MFA TOTP / FIDO2)"]

    Prog --> Keys["Access Key ID & Secret Access Key<br>(AWS CLI, SDKs, APIs)"]
    Prog --> SSH["SSH Public Keys<br>(AWS CodeCommit)"]
    Prog --> Cert["X.509 Certificates<br>(SOAP / Signing legacy requests)"]

    Temp --> STS["AWS STS Session Tokens<br>(IAM Roles, Federation, EC2 Profiles)"]
```

### 2.1 Credential Types and Intended Use Cases

#### 1. AWS Account Root User Credentials (Email Address & Password)
- **Association**: Directly associated with the contract owner of the AWS account.
- **Capabilities**: Unrestricted, omnipotent access to every resource, billing account, and configuration setting. Root permissions cannot be limited or constrained by IAM policies.
- **Operational Rule**: **Never use root credentials for daily administrative or development work.** After account setup, lock root credentials away in a secure vault.

#### 2. IAM User Name and Password
- **Association**: Created for individual human team members.
- **Intended Use**: Signing in to the interactive **AWS Management Console** via an account-specific sign-in URL (`https://<account-id-or-alias>.signin.aws.amazon.com/console`).
- **Security Control**: Enforced through account password policies and mandatory Multi-Factor Authentication (MFA).

#### 3. Access Key Pairs (Access Key ID & Secret Access Key)
- **Association**: Generated for IAM users or programmatic service identities.
- **Intended Use**: Authenticating programmatic requests via the **AWS Command Line Interface (AWS CLI)**, **AWS Software Development Kits (SDKs)**, and direct REST API invocations.
- **Components**:
  - **Access Key ID** (e.g., `AKIAIOSFODNN7EXAMPLE`): A 20-character alphanumeric identifier that acts like a public username.
  - **Secret Access Key** (e.g., `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`): A 40-character cryptographically generated string that acts like a password. AWS uses this key to calculate an HMAC-SHA256 signature (Signature Version 4) for every API request.
- **Critical Caution**: The Secret Access Key can only be viewed or downloaded once at the moment of creation. If lost, it cannot be recovered; it must be deactivated, deleted, and regenerated.

> [!CAUTION]
> **Never embed long-term access keys in source code, GitHub repositories, or application container images.** If committed to public repositories, automated bot scanners will compromise the keys within seconds, leading to resource theft and cryptocurrency mining exploits.

#### 4. Multi-Factor Authentication (MFA)
- **Association**: Configurable for both the root user and individual IAM users.
- **Mechanism**: Adds a mandatory second authentication factor on top of passwords (combining *something you know* with *something you have*).
- **Supported Authenticators**:
  - **FIDO2 / WebAuthn Hardware Security Keys**: YubiKey, Feitian (highest security; phish-resistant).
  - **Virtual MFA Apps**: Smartphone TOTP apps (Google Authenticator, Microsoft Authenticator, Authy).
  - **Hardware TOTP Key Fobs**: Dedicated physical tokens displaying time-synchronized numeric codes.
  - **SMS-based MFA**: Supported for root and IAM, but deprecated in enterprise security standards due to vulnerabilities to SIM-swapping attacks.

#### 5. Key Pairs (Public Key & Private Key)
- **Association**: Used by specific AWS compute services, predominantly **Amazon EC2**.
- **Intended Use**: Cryptographically authenticating SSH connections to Linux instances (using the private key `.pem` file) or decrypting the initial Administrator password for Windows EC2 instances. Key pairs are distinct from IAM access keys.

#### 6. SSH Keys for AWS CodeCommit
- **Association**: Uploaded to an IAM user profile.
- **Intended Use**: Authenticating Git version control operations directly against AWS CodeCommit repositories over SSH.

#### 7. X.509 Signing Certificates
- **Association**: Uploaded to IAM for specific programmatic verification.
- **Intended Use**: Digitally signing certain API requests, SOAP endpoints, or client VPN connections.

#### 8. AWS STS Temporary Security Credentials
- **Association**: Issued dynamically by the AWS Security Token Service (AWS STS).
- **Components**: Access Key ID (beginning with `ASIA...`), Secret Access Key, and a mandatory **Session Token**.
- **Lifecycle**: Automatically expire after a configurable duration (from 15 minutes up to 36 hours). Used by IAM Roles, eliminating long-term credential storage on servers.

---

### 2.2 Security Credentials Comparison Matrix

| Credential Type | Form Factor | Access Method | Target Audience / Use Case | Lifespan & Rotation |
| :--- | :--- | :--- | :--- | :--- |
| **Root User Password** | Email + Password string | Web Console | Account initialization, billing changes, break-glass recovery | Long-term; rotate periodically |
| **IAM User Password** | Alphanumeric string | Web Console | Daily human engineers, operators, cloud developers | Subject to account password expiration policies |
| **Access Key Pair** | Key ID (`AKIA...`) + Secret String | CLI, SDK, API | Local developer CLI tooling, on-premises external scripts | Long-term; requires manual rotation every 90 days |
| **MFA Code** | 6-digit dynamic TOTP or cryptographic token | Web Console & CLI | Added security layer for all interactive human logins | 30-second validity per code (TOTP) |
| **EC2 Key Pair** | RSA / ED25519 asymmetric key pair | SSH / RDP | Operating system shell access to Amazon EC2 virtual machines | Long-term; managed by instance administrator |
| **STS Session Credentials** | Key ID (`ASIA...`) + Secret + Session Token | CLI, SDK, API | IAM Roles, EC2 Instance Profiles, Lambda runtimes, Federated SSO | Ephemeral (15 min – 12/36 hrs); automated rotation |

---

## 3. IAM Entities: Root, Users, Groups, and Roles

IAM structures access governance around four primary entity types: the **Root User**, **IAM Users**, **IAM User Groups**, and **IAM Roles**.

```mermaid
flowchart TD
    subgraph AWSAccount["🏢 AWS Account Domain"]
        Root["👑 AWS Account Root User<br>(Full Unrestricted Power)"]
        
        subgraph IAMUsers["IAM Users (Identities)"]
            UserA["👤 User: Li"]
            UserB["👤 User: Mateo"]
            UserC["👤 User: Ana"]
            UserD["👤 User: Paulo"]
        end

        subgraph IAMGroups["IAM User Groups (Permission Collections)"]
            GrpAdmin["👥 Admins Group"]
            GrpDev["👥 Developers Group"]
            GrpTest["👥 Test Group"]
        end

        subgraph IAMRoles["IAM Roles (Temporary Delegation)"]
            RoleEC2["⚙️ EC2-S3-Access-Role"]
            RoleFed["🛡️ CrossAccount-Audit-Role"]
        end
    end

    GrpAdmin -->|Members| UserA
    GrpDev -->|Members| UserB
    GrpDev -->|Members| UserC
    GrpTest -->|Members| UserD

    PolA["📜 Admin Policy"] --> GrpAdmin
    PolB["📜 S3 Read Policy"] --> GrpDev
    PolC["📜 Test Policy"] --> GrpTest

    RoleEC2 -.->|Assumed by Instance Profile| Server["🖥️ EC2 Workload"]
    RoleFed -.->|Assumed by STS| ExtUser["👤 External Auditor"]
```

### 3.1 The AWS Account Root User

When an AWS account is first created, it begins with a single omnipotent identity: the **AWS Account Root User**.
- **Sign-in Identifier**: The email address used to register the AWS account.
- **Authority**: Has unconditional, complete access to all AWS resources, API actions, and billing records in the account.
- **Irrevocable Permissions**: The permissions of the root user **cannot be restricted by any IAM policy** attached within the account.

```mermaid
graph LR
    RootUser["👑 Account Root User<br>(Unconstrained)"]
    InitTask["1. Create Initial Admin IAM User"]
    SecureRoot["2. Enable MFA & Secure Root Credentials"]
    DailyOps["3. Perform All Daily Work via IAM Admin / SSO"]

    RootUser --> InitTask --> SecureRoot --> DailyOps
```

> [!IMPORTANT]
> **Root User Protective Imperatives**:
> 1. Never generate Access Keys for the root user. If root access keys exist, delete them immediately.
> 2. Enable hardware or virtual MFA on the root user immediately.
> 3. Store the root password and physical MFA device in a secure corporate vault.
> 4. Restrict root logins exclusively to tasks that strictly require root privileges:
>    - Closing an AWS account.
>    - Changing the AWS account name, root email, or root password.
>    - Changing or canceling AWS Support plans.
>    - Registering as an AWS GovCloud seller.
>    - Restoring permissions for an IAM user who accidentally revoked their own admin rights.

---

### 3.2 IAM Users

An **IAM user** is an identity entity created inside an AWS account that represents a specific person or application requiring interaction with AWS resources.
- **Identity & Credentials**: An IAM user consists of a friendly name (e.g., `Martha`, `DevApp1`) and assigned credentials (password for console, access keys for API/CLI).
- **Default State**: A newly created IAM user has **no default security credentials** and **zero permissions**. By default, an IAM user cannot sign in and cannot perform any action on any resource (enforcing implicit deny).
- **Human vs. Machine**: While IAM users can represent automated applications, AWS best practices strongly recommend using **IAM Roles** instead of IAM users for applications running inside AWS infrastructure.

---

### 3.3 IAM User Groups

An **IAM User Group** is a collection of IAM users. It serves as an administrative management container to apply policies across multiple users simultaneously.

#### Critical Characteristics of Groups:
1. **Convenience Container**: Groups simplify permission management. Instead of attaching policies to ten individual developers, administrators attach the policy once to the `Developers` group.
2. **No Nesting**: **IAM groups cannot be nested.** A group can only contain users; it can never contain other groups (e.g., `JuniorDevelopers` cannot be a member of `Developers`).
3. **Multi-Group Membership**: An IAM user can belong to multiple groups simultaneously (up to 300 groups by default). The user inherits the cumulative permissions granted by all groups to which they belong.
4. **Not a True Identity / Principal**: A group is not an identity that can be authenticated. A group has no credentials, cannot sign in, and **cannot be referenced as a `Principal`** in resource-based policies.
5. **No Default Group**: There is no default system group that automatically includes all users in an account.

```mermaid
flowchart TD
    subgraph MultiGroupExample["Multi-Group Permission Inheritance"]
        User["👤 User: Sofia"]
        GrpDev["👥 Group: Developers<br>(Policy: CodeCommit Read/Write)"]
        GrpQA["👥 Group: QA-Testers<br>(Policy: S3 Test-Bucket Access)"]

        GrpDev -->|Member| User
        GrpQA -->|Member| User

        User ==> Perms["Sofia's Effective Permissions:<br>• CodeCommit Read/Write<br>• S3 Test-Bucket Access"]
    end
```

---

### 3.4 IAM Roles and Temporary Access via AWS STS

An **IAM Role** is an identity that you create in your account that has specific permissions, but is **not associated with a specific person or long-term credentials**. Instead, trusted entities assume the role dynamically at runtime to obtain temporary security credentials generated by **AWS Security Token Service (AWS STS)**.

```mermaid
sequenceDiagram
    autonumber
    actor Workload as 🖥️ Amazon EC2 (Application)
    participant STS as ⏳ AWS STS (Security Token Service)
    participant IAM as 🛡️ AWS IAM
    participant S3 as 🪣 Amazon S3

    Note over Workload,IAM: IAM Role: "S3ReadOnlyRole" Attached via Instance Profile
    Workload->>STS: AssumeRole API Call (Request temporary credentials)
    STS->>IAM: Check Role Trust Policy (Can EC2 assume this role?)
    IAM-->>STS: Trust Validated ✅
    STS-->>Workload: Temporary Credentials (Key ID, Secret Key, Session Token, Expiry)
    Workload->>S3: GET s3://finance-data/report.csv (Signed with temporary STS key)
    S3->>IAM: Evaluate Permissions Policy
    IAM-->>S3: Allow ✅
    S3-->>Workload: HTTP 200 OK (Data Stream)
```

#### Anatomy of an IAM Role:
Every IAM role is defined by two distinct JSON policies:

1. **Trust Policy (AssumeRole Policy)**:
   - Defines **who** is permitted to assume the role (the trusted entity/Principal).
   - Specifies whether the caller can be an AWS service (e.g., `ec2.amazonaws.com`), an external AWS account ID, a SAML identity provider, or an OIDC web provider.
2. **Permissions Policy**:
   - Defines **what** actions the role can perform on which resources once assumed.

#### Primary Use Cases for IAM Roles:
- **Workloads on Compute Services**: Granting an EC2 instance, ECS container, or AWS Lambda function permissions to access S3 or DynamoDB without embedding access keys in code or configuration files.
- **Cross-Account Access**: Allowing administrators or automated auditors in Account A to access resources in Account B without duplicating user accounts.
- **Identity Federation**: Allowing enterprise users authenticated by Microsoft Active Directory, Okta, or Google Workspace to assume an IAM role and access AWS resources using SAML 2.0 or OIDC.

---

### 3.5 IAM Entities Comparison Matrix

| Property | AWS Account Root User | IAM User | IAM User Group | IAM Role |
| :--- | :--- | :--- | :--- | :--- |
| **Entity Definition** | Master account identity created with account | Individual identity created within account | Administrative collection of IAM users | Assignable identity assumed dynamically |
| **Credentials** | Email + Password, optional MFA | Password, Access Keys, optional MFA | **None** (No credentials exist) | **None** (Issues temporary STS credentials upon assumption) |
| **Long-Term Secrets** | Yes | Yes (if access keys generated) | No | **No** (Ephemeral credentials only) |
| **Can Be Principal?** | Yes | Yes | **No** (Cannot be named in `Principal`) | Yes |
| **Nesting Supported?** | N/A | N/A | **No** (Groups cannot contain groups) | N/A |
| **Recommended Usage** | Account initialization, billing, account closure | Humans requiring explicit AWS console login | Organizing users by job function / department | Compute workloads, cross-account access, federation |

---

## 4. IAM Policies & JSON Syntax

Permissions in AWS are formalized using **IAM Policies**. An IAM policy is a formal JSON (JavaScript Object Notation) document that defines permissions by explicitly specifying what actions are allowed or denied.

### 4.1 JSON Policy Document Grammar & Elements

```mermaid
classDiagram
    class PolicyDocument {
        +String Version
        +Statement[] Statement
    }
    class Statement {
        +String Sid
        +String Effect
        +Principal Principal
        +String[] Action
        +String[] NotAction
        +String[] Resource
        +String[] NotResource
        +Condition Condition
    }
    PolicyDocument *-- Statement
```

A policy document contains the following core structural elements:

1. **`Version`**:
   - Specifies the language syntax rules.
   - **Always use `"2012-10-17"`**. (An older version `"2008-10-17"` exists but lacks support for advanced policy variables and conditions).
2. **`Statement`**:
   - The primary container element. Contains one or more individual permission statements.
3. **`Sid` (Statement ID)**:
   - An optional human-readable identifier for the statement (e.g., `"AllowDynamoAndS3Read"`).
4. **`Effect`**:
   - Required. Dictates whether the statement results in an explicit allow or an explicit deny. Valid values are strictly `"Allow"` or `"Deny"`.
5. **`Principal`**:
   - Specifies the user, account, service, or federated entity allowed or denied access.
   - **Required in Resource-Based Policies and Role Trust Policies.**
   - **Not allowed in Identity-Based Policies** (because the principal is implicitly the entity to which the policy is attached).
6. **`Action` / `NotAction`**:
   - Specifies the specific API operations allowed or denied. Formatted as `<service>:<actionName>` with wildcard support (e.g., `"s3:GetObject"`, `"dynamodb:*"`, `"ec2:Describe*"`).
   - `NotAction` matches everything *except* the listed actions.
7. **`Resource` / `NotResource`**:
   - Specifies the target AWS objects using their **Amazon Resource Names (ARNs)** (e.g., `"arn:aws:s3:::my-corporate-bucket/*"`).
   - `NotResource` matches all resources *except* the explicitly specified ARNs.
8. **`Condition`**:
   - An optional block specifying contextual constraints when the policy is in effect (e.g., enforcing SSL via `aws:SecureTransport`, restricting by IP range via `aws:SourceIp`, or requiring MFA via `aws:MultiFactorAuthPresent`).

---

### 4.2 Dissecting the Module Example Policy

The curriculum slide presents a classic multi-statement JSON policy demonstrating both `Allow` and `Deny` statements with `Resource` and `NotResource`:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ExplicitAllowDynamoAndS3",
      "Effect": "Allow",
      "Action": [
        "DynamoDB:*",
        "s3:*"
      ],
      "Resource": [
        "arn:aws:dynamodb:region:account-number-withouth-hypens:table/table-name",
        "arn:aws:s3:::bucket-name",
        "arn:aws:s3:::bucket-name/*"
      ]
    },
    {
      "Sid": "ExplicitDenyAllOtherResources",
      "Effect": "Deny",
      "Action": [
        "DynamoDB:*",
        "s3:*"
      ],
      "NotResource": [
        "arn:aws:dynamodb:region:account-number-withouth-hypends:table/table-name",
        "arn:aws:s3:::bucket-name",
        "arn:aws:s3:::bucket-name/*"
      ]
    }
  ]
}
```

#### Detailed Policy Breakdown:
- **Statement 1 (`ExplicitAllowDynamoAndS3`)**:
  - Grants permission to perform all DynamoDB actions (`DynamoDB:*`) and all S3 actions (`s3:*`).
  - Restricts these actions strictly to the specified DynamoDB table, the specified S3 bucket, and all objects within that bucket (`bucket-name/*`).
- **Statement 2 (`ExplicitDenyAllOtherResources`)**:
  - Contains an `Effect: Deny`.
  - Uses the **`NotResource`** element with the exact same resource list.
  - **Effect**: Explicitly denies any DynamoDB or S3 action against *any other table or bucket* in the entire AWS universe.
  - **Why this matters**: In AWS IAM, an **Explicit Deny always overrides an Allow**. Even if another policy attached to this user (such as `AdministratorAccess` or `AmazonS3FullAccess`) grants broad access to other buckets, this explicit deny statement ensures the user can touch *only* this specific table and bucket.

---

### 4.3 Classification of IAM Policies

Policies are categorized based on how and where they are attached:

```mermaid
flowchart TD
    PolType["📜 IAM Policy Classification"]
    
    IdBased["Identity-Based Policies<br>(Attached to Users, Groups, Roles)"]
    ResBased["Resource-Based Policies<br>(Attached directly to AWS Resources)"]

    PolType --> IdBased
    PolType --> ResBased

    IdBased --> Managed["Managed Policies<br>(Reusable standalone JSON docs)"]
    IdBased --> Inline["Inline Policies<br>(Embedded 1:1 in identity)"]

    Managed --> AWSManaged["AWS Managed Policies<br>(Created & maintained by AWS)"]
    Managed --> CustManaged["Customer Managed Policies<br>(Created & versioned by customer)"]
```

#### 1. Identity-Based Policies
Attached to an IAM identity (a user, a group, or a role).
- **AWS Managed Policies**: Standalone policies created, maintained, and updated by AWS (e.g., `AdministratorAccess`, `AmazonS3ReadOnlyAccess`). Customers cannot edit these policies.
- **Customer Managed Policies**: Standalone policies created and administered in your own AWS account. They offer full versioning (up to 5 versions) and can be attached to multiple entities simultaneously.
- **Inline Policies**: Policies embedded directly into a single specific IAM user, group, or role. When the identity is deleted, the inline policy is deleted with it. Inline policies lack reusability and version control.

#### 2. Resource-Based Policies
JSON policies attached directly to an AWS resource rather than to an IAM identity.
- Common examples: **Amazon S3 Bucket Policies**, **AWS KMS Key Policies**, **Amazon SQS Queue Policies**, and **AWS Lambda Function Policies**.
- Resource-based policies specify permissions for a specified `Principal`.
- **Key Architecture Note**: All resource-based policies are inline policies. There are no managed resource-based policies.
- Resource-based policies enable cross-account access without requiring the caller to assume an IAM role.

---

### 4.4 Policy Types Comparison Table

| Feature | AWS Managed Policy | Customer Managed Policy | Inline Policy | Resource-Based Policy |
| :--- | :--- | :--- | :--- | :--- |
| **Attachment Target** | Users, Groups, Roles | Users, Groups, Roles | Single User, Group, or Role | AWS Resources (S3, KMS, SQS) |
| **Managed By** | AWS | Customer | Customer | Customer |
| **Reusability** | Attach to multiple entities | Attach to multiple entities | Strictly 1-to-1 embedded | Single resource |
| **Version History** | Maintained by AWS | Up to 5 versions with rollback | No versioning | Managed on the resource |
| **Contains `Principal`?** | No | No | No | **Yes (Mandatory)** |
| **Best Use Case** | Broad job-function roles | Custom enterprise least privilege | Strict 1:1 binding (rare) | Cross-account bucket sharing, KMS keys |

---

## 5. The IAM Policy Evaluation Engine

When an authenticated identity submits a request to AWS, the IAM policy evaluation engine executes a deterministic evaluation algorithm to render an **Allow** or **Deny** decision.

### 5.1 The Fundamental Axioms of IAM Evaluation

1. **Default Deny (Implicit Deny)**: By default, all requests are denied. If no policy statements apply to a request, it is denied.
2. **Explicit Allow Requirement**: A request is permitted if and only if at least one applicable policy statement contains an explicit `Effect: "Allow"`.
3. **Explicit Deny Precedence**: An explicit `Effect: "Deny"` in any applicable policy **always beats everything else**. If an explicit deny matches the request, the request is immediately denied, regardless of how many allow statements exist.
4. **Order Independence**: The evaluation engine evaluates all applicable policies across all scopes. The order in which policies are evaluated does not change the outcome.

---

### 5.2 Deterministic Decision Tree Flowchart

```mermaid
flowchart TD
    Start(["🚀 Incoming Authenticated Request<br>(Principal, Action, Resource, Context)"]) --> CheckDeny{"Is there an<br>EXPLICIT DENY<br>in ANY applicable policy?"}

    CheckDeny -- YES --> DenyResult["🚫 FINAL DECISION: DENY<br>(Explicit Deny overrides all)"]
    
    CheckDeny -- NO --> CheckAllow{"Is there an<br>EXPLICIT ALLOW<br>in ANY applicable policy?"}

    CheckAllow -- YES --> AllowResult["✅ FINAL DECISION: ALLOW<br>(Authorized to execute API action)"]
    
    CheckAllow -- NO --> ImplicitDeny["🚫 FINAL DECISION: DENY<br>(Default / Implicit Deny)"]

    style DenyResult fill:#f88,stroke:#c00,stroke-width:2px;
    style ImplicitDeny fill:#fbb,stroke:#d33,stroke-width:2px;
    style AllowResult fill:#8f8,stroke:#090,stroke-width:2px;
```

> [!IMPORTANT]
> **Summary Formula**:
> $$\text{Final Decision} = (\text{Explicit Deny} \implies \text{DENY}) \lor (\text{Explicit Allow} \implies \text{ALLOW}) \lor \text{DENY (Default)}$$

---

## 6. AWS IAM Security Best Practices

The AWS Well-Architected Framework and security benchmarks mandate eight operational best practices when configuring IAM:

```mermaid
flowchart TD
    BP["🛡️ AWS IAM Best Practice Pillars"]

    BP --> BP1["1. Lock Root Credentials<br>Delete root access keys; enable root MFA"]
    BP --> BP2["2. Individual IAM Identities<br>Never share accounts; distinct credentials"]
    BP --> BP3["3. Group-Based Permissions<br>Attach policies to groups, not individual users"]
    BP --> BP4["4. Principle of Least Privilege<br>Grant only minimum necessary permissions"]
    BP --> BP5["5. Enforce Strong Password Policy<br>Minimum length, complexity, age expiration"]
    BP --> BP6["6. Enforce MFA Everywhere<br>Mandatory for all console and privileged users"]
    BP --> BP7["7. Use Roles for Workloads & Federation<br>Eliminate long-term keys on EC2 & containers"]
    BP --> BP8["8. Regularly Rotate & Audit Credentials<br>Retire unused keys; test via Policy Simulator"]
```

### 6.1 Lock Away AWS Account Root User Access Keys
- Never create or retain access keys for the root user.
- If root access keys currently exist in your account, immediately deactivate and permanently delete them.
- Protect root login with hardware or virtual MFA.

### 6.2 Create Individual IAM Users
- Never allow team members to share an administrative or developer account.
- Shared accounts eliminate accountability and destroy **non-repudiation** in AWS CloudTrail logs.
- If a team member leaves the company, individual credential deletion ensures zero disruption to the remaining staff.

### 6.3 Use User Groups to Assign Permissions
- Attach managed policies to user groups rather than attaching policies directly to individual users.
- As personnel change roles or join/leave teams, administrators simply modify group memberships rather than editing multiple distinct permission documents.

### 6.4 Grant Least Privilege
- Start with minimum permissions and grant additional access only as business requirements demand.
- Avoid broad administrative wildcards (`"Action": "*"` and `"Resource": "*"`) in production environments.
- Scope down resources to specific ARNs and limit actions to specific API calls.

### 6.5 Configure a Strong Password Policy
- Enforce enterprise password standards across the AWS account:
  - Minimum length (at least 14 characters recommended by modern NIST standards).
  - Require a mix of character types (uppercase, lowercase, numbers, non-alphanumeric symbols).
  - Enforce password rotation and prevent password reuse.
  - Enable automatic password expiration.

### 6.6 Enforce Multi-Factor Authentication (MFA)
- Require MFA for all users with access to the AWS Management Console.
- Enforce MFA via policy conditions (`"Condition": {"Bool": {"aws:MultiFactorAuthPresent": "true"}}`) so that users without an active MFA token cannot perform sensitive API operations even if they possess valid access keys.

### 6.7 Use IAM Roles for Workloads and Identity Federation
- Never store static access keys inside configuration files, application code, or Amazon EC2 instance storage.
- Assign an **IAM Role** to EC2 instances using an **Instance Profile**. The AWS SDK running on the instance will automatically fetch temporary STS credentials and rotate them seamlessly.
- Connect existing corporate Active Directory or Okta directories using IAM identity federation rather than creating duplicate IAM users.

### 6.8 Rotate Credentials Regularly & Remove Unused Credentials
- Establish an automated 90-day rotation cadence for all long-term IAM access keys.
- Run the IAM Credential Report regularly to identify inactive credentials, keys older than 90 days, or users who have not logged in recently.

### 6.9 Validate and Test with the IAM Policy Simulator
- Before deploying policies to production, test and troubleshoot identity-based and resource-based policies using the **IAM Policy Simulator**.
- Verify that explicit denies behave as expected and that least privilege boundaries are not breached.

---

## 7. Practical Walkthrough: IAM Console & Administration

Based on the classroom console demonstration, follow these operational procedures to initialize, configure, and verify an IAM security baseline.

### 7.1 Accessing the IAM Dashboard
1. Open the [AWS Management Console](https://console.aws.amazon.com/).
2. In the top search bar, type `IAM` and select **IAM (Manage access to AWS resources)**.
3. Observe that the Region selector in the navigation bar automatically switches to **Global** (IAM does not require Region selection).
4. Review the **Security recommendations** dashboard (e.g., *Add MFA for root user*, *Manage access keys*, *Set password policy*).

```mermaid
flowchart LR
    ConsoleSearch["🔍 Search 'IAM'"] --> GlobalDashboard["🌐 Global IAM Console"]
    GlobalDashboard --> CheckRecs["📋 Review Security Recommendations"]
    CheckRecs --> CreateAdmin["👤 Create Initial Admin User"]
    CreateAdmin --> LockRoot["🔒 Lock Root User & Enable MFA"]
```

### 7.2 Creating an Administrative IAM User & User Group
To avoid using root credentials for future administration, configure an initial administrator:
1. In the IAM navigation pane, choose **User groups** $\rightarrow$ **Create group**.
2. **User group name**: Enter `Admins`.
3. Under **Attach permissions policies**, filter for `AdministratorAccess` (an AWS managed policy).
4. Select the checkbox next to `AdministratorAccess` and click **Create group**.
5. In the navigation pane, choose **Users** $\rightarrow$ **Add users**.
6. **User details**:
   - Username: `admin-lead`.
   - Provide user access to the AWS Management Console: Select **Enable**.
   - Select **I want to create an IAM user**.
   - Choose **Custom password** or **Autogenerated password**.
   - Check **Users must create a new password at next sign-in**.
7. **Set permissions**: Add the user to the newly created `Admins` group.
8. Complete creation and record the user's customized sign-in URL:
   `https://<account-id>.signin.aws.amazon.com/console`
9. Sign out of root, sign in as `admin-lead`, and enable MFA immediately.

---

## 🧠 8. Checkpoint Review & Knowledge Check

Test your comprehension against the curriculum checkpoint questions and advanced certification scenarios:

### Question 1: What are two ways that you can use IAM to access AWS services?
> **Answer**:
> You can access AWS services through:
> 1. **The AWS Management Console**: An interactive graphical user interface accessed via web browsers using an IAM username and password (plus MFA).
> 2. **Programmatic Access**: Invoking AWS APIs through the **AWS Command Line Interface (AWS CLI)**, **Software Development Kits (SDKs)**, or direct REST API calls authenticated using an **Access Key ID and Secret Access Key** (or temporary STS credentials).

### Question 2: What are some examples of types of security credentials supported by AWS?
> **Answer**:
> Supported security credentials include:
> - **Email address and password**: Associated with the AWS account root user.
> - **IAM user name and password**: Used for human console sign-in.
> - **Access Key ID and Secret Access Key**: Used for programmatic CLI and SDK requests.
> - **Multi-Factor Authentication (MFA)**: Hardware tokens, virtual TOTP apps, or FIDO2 keys providing a second factor.
> - **Key Pairs**: Public/private key pairs used for SSH access to Amazon EC2 Linux instances and Windows password decryption.
> - **SSH Keys for AWS CodeCommit**: Git repository authentication.
> - **X.509 Certificates**: Cryptographic signing for SOAP or legacy endpoints.
> - **Temporary Security Credentials**: Session tokens generated by AWS STS.

### Question 3: What is the difference between an IAM user, an IAM role, and an IAM group?
> **Answer**:
> - **IAM User**: A persistent identity created within an AWS account that represents a specific person or application, possessing permanent long-term credentials (password, access keys) and attached permissions.
> - **IAM Group**: A management collection of IAM users. Permissions attached to a group are inherited by all member users. Groups cannot contain other groups and have no credentials of their own.
> - **IAM Role**: An identity with permissions that is not tied to a specific person and has no long-term credentials. It is assumed temporarily by authorized principals (such as EC2 instances, Lambda functions, or federated enterprise users) via AWS STS.

### Question 4: A developer is assigned two policies. Policy A grants `Allow` for `s3:*` on all buckets. Policy B contains `Deny` for `s3:DeleteBucket` on all buckets. Can the developer delete an S3 bucket? Explain the evaluation mechanic.
> **Answer**:
> **No, the developer cannot delete the bucket.**  
> In IAM policy evaluation, an **Explicit Deny always takes precedence over an Explicit Allow**. Because Policy B contains an explicit deny for `s3:DeleteBucket`, the evaluation engine terminates with a `Deny` decision for that action, regardless of Policy A's allow statement.

### Question 5: Why is an IAM User Group prohibited from being specified as the `Principal` in an Amazon S3 Bucket Policy?
> **Answer**:
> An IAM User Group is purely an administrative container for managing user permissions within an AWS account; it is **not an authenticatable identity or principal**. IAM groups do not possess an Amazon Resource Name (ARN) that can be authenticated, cannot hold credentials, and cannot make API requests. Resource-based policies can only list valid Principals (IAM users, IAM roles, AWS accounts, or AWS services).

---

## 📌 9. Key Takeaways & Exam Summary

- **IAM Definition & Core Nature**:
  - Centralized security service for managing authentication and authorization.
  - Operates **globally** across all AWS Regions; zero configuration per Region.
  - Free service with **zero additional charges**.
- **Authentication vs. Authorization**:
  - Authentication (AuthN) proves *who you are* (passwords, access keys, MFA).
  - Authorization (AuthZ) governs *what you can do* (JSON policies).
- **Credentials Summary**:
  - Passwords $\rightarrow$ Console access.
  - Access Key Pairs $\rightarrow$ CLI / SDK / programmatic API access.
  - Key Pairs $\rightarrow$ EC2 OS login.
  - STS Tokens $\rightarrow$ Temporary session credentials.
- **Root User Protocol**:
  - Has complete, unconstrained administrative power.
  - Lock away credentials; delete root access keys; enable MFA; restrict to break-glass tasks.
- **Entity Guidelines**:
  - **IAM User**: Represents individual person or app; no default permissions.
  - **IAM Group**: Collection of users; no nesting; simplifies permission reuse.
  - **IAM Role**: Temporary credentials via AWS STS; no long-term keys; ideal for EC2 and federation.
- **Policy Evaluation Engine Rules**:
  - **Default Deny**: All requests start denied.
  - **Explicit Allow**: Required to grant access.
  - **Explicit Deny**: Beats any and all allows unconditionally.
- **JSON Structure**:
  - `Version`: `"2012-10-17"`.
  - Core fields: `Statement`, `Sid`, `Effect`, `Principal`, `Action`, `Resource`, `Condition`.

---

## 📚 10. Authoritative References & Documentation

- [AWS Identity and Access Management User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
- [Policies and Permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)
- [IAM Policy Evaluation Logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)
- [Security Best Practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [Testing IAM Policies with the IAM Policy Simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testingpolicies.html)
- [AWS Security Token Service (STS) Documentation](https://docs.aws.amazon.com/STS/latest/UsingSTS/Welcome.html)
