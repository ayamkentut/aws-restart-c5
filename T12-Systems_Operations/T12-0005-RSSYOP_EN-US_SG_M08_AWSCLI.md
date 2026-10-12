# 💻 AWS Command Line Interface (AWS CLI): Architecture, Installation, Configuration, & Operational Automation

> **Course**: AWS re/Start  
> **Module**: Systems Operations (M08 – Systems Operations on AWS / AWS CLI)  
> **Source Document**: `T12-Systems_Operations/T12-0005-RSSYOP_EN-US_SG_M08_AWSCLI.pdf`  
> **Target Audience**: AWS re/Start Learners, Systems Administrators, Cloud Operations Engineers, SysOps Administrators  

---

## 🎯 Module Overview & Systems Operations Context

In modern enterprise cloud operations, efficiency, repeatability, and precision are paramount. While graphical user interfaces (GUIs) like the AWS Management Console provide an accessible environment for initial exploration and visual inspection, running infrastructure at scale demands automation. Manual configuration via the console—often termed "ClickOps"—is prone to human error, configuration drift, and lack of reproducibility.

The **AWS Command Line Interface (AWS CLI)** is an open-source, unified command-line tool developed and maintained by AWS. It enables cloud engineers, systems administrators, and DevOps practitioners to manage, automate, and orchestrate all AWS services directly from the terminal or through shell scripts. 

For Systems Operations (SysOps) professionals, the AWS CLI bridges the gap between ad-hoc infrastructure manipulation and declarative Infrastructure as Code (IaC). It provides rapid diagnostic capabilities, scriptable operational maintenance routines, CI/CD pipeline automation hooks, and high-performance bulk operations across globally distributed AWS resources.

```mermaid
flowchart TD
    subgraph ManagementTiers["Operational Management Tiers"]
        Console["🖥️ AWS Management Console<br>(Web Browser GUI - Ad-hoc & Visual)"]
        CLI["💻 AWS CLI<br>(Terminal & Shell Scripts - Interactive & Automated)"]
        SDK["⚙️ AWS SDKs<br>(Python, Java, Go, JS - Embedded in Applications)"]
    end

    subgraph CoreEngine["Unified AWS Infrastructure Layer"]
        RestAPI["🌐 AWS Service REST APIs<br>(HTTPS Endpoints, SigV4 Authentication)"]
        Services["☁️ Target AWS Services<br>(Amazon EC2, Amazon S3, AWS IAM, Amazon VPC, RDS)"]
    end

    Console -->|Underlying REST Calls| RestAPI
    CLI -->|Signed REST API Requests| RestAPI
    SDK -->|Programmatic REST Calls| RestAPI
    RestAPI --> Services

    style CLI fill:#232F3E,stroke:#FF9900,stroke-width:2px,color:#FFFFFF
    style RestAPI fill:#FF9900,stroke:#232F3E,stroke-width:2px,color:#000000
```

---

### 📌 Learning Objectives

- [ ] **Define the AWS Command Line Interface (AWS CLI)**: Understand the role and architectural advantages of the unified CLI tool in systems administration.
- [ ] **Distinguish the Three Ways to Access AWS**: Compare the AWS Management Console, the AWS CLI, and AWS Software Development Kits (SDKs), recognizing how each interacts with the underlying AWS REST API.
- [ ] **Compare AWS CLI Versions (v1 vs. v2)**: Identify the architectural improvements, packaging models, and features introduced in AWS CLI version 2, including self-contained runtime bundles, auto-prompting, and AWS IAM Identity Center (AWS SSO) support.
- [ ] **Execute Installation, Verification, and Upgrades on Linux**: Perform end-to-end command-line installation using `curl`, `unzip`, and `sudo ./aws/install`, verify the deployment via `aws --version`, and execute in-place updates.
- [ ] **Configure Credentials and Profiles**: Execute the interactive `aws configure` wizard, manage the four foundational settings (Access Key ID, Secret Access Key, Default Region, Output Format), and navigate the distinct roles of `~/.aws/credentials` and `~/.aws/config`.
- [ ] **Deconstruct AWS CLI Command Anatomy**: Master the syntax structure: `aws <service> <operation> [parameters] [options]`, including passing complex parameters via JSON documents.
- [ ] **Navigate the In-Terminal Help System**: Query manual pages and command documentation interactively via `aws help`, `aws <service> help`, and `aws <service> <operation> help`.
- [ ] **Differentiate Server-Side Filtering from Client-Side Querying**: Contrast server-side efficiency using `--filter` against client-side parsing using JMESPath expressions with `--query`.
- [ ] **Perform Safe Operations with Dry-Run Validation**: Validate IAM permissions and parameter syntax defensively using the `--dry-run` flag before executing modifying state changes.
- [ ] **Execute Foundational SysOps Commands**: Administer Amazon EC2 instances and Amazon S3 object storage workloads using standard high-level and operational CLI syntax.
- [ ] **Implement Programmatic Security Best Practices**: Enforce least privilege, eliminate hardcoded access keys, leverage EC2 instance profiles and IAM roles, and respect configuration precedence hierarchies.

---

## 🌐 1. Three Ways to Access AWS & The Underlying REST API

Every action executed within Amazon Web Services—whether launching an Amazon EC2 instance, creating an Amazon S3 bucket, or modifying an IAM policy—is fundamentally an **HTTP/HTTPS request made to an AWS REST-like Application Programming Interface (API) endpoint**.

AWS abstracts direct raw HTTP calls behind three primary operational interfaces:

```mermaid
flowchart LR
    subgraph Clients["Operator & Workload Interfaces"]
        direction TB
        C1["AWS Management Console<br>• Visual web GUI<br>• Point-and-click operations"]
        C2["AWS CLI<br>• Terminal commands<br>• Bash / PowerShell scripts"]
        C3["AWS SDKs<br>• Application code<br>• Boto3, Java, Node.js, Go"]
    end

    subgraph AuthLayer["Signing & Transport Layer"]
        direction TB
        SigV4["AWS Signature Version 4 (SigV4)<br>• HMAC-SHA256 signature<br>• Time-stamped & authenticated"]
        TLS["HTTPS / TLS 1.2+ Transport<br>• Encrypted endpoint transmission"]
    end

    subgraph Endpoints["Regional AWS Service APIs"]
        direction TB
        EC2Api["ec2.us-east-1.amazonaws.com"]
        S3Api["s3.us-east-1.amazonaws.com"]
        IAMApi["iam.amazonaws.com (Global)"]
    end

    C1 --> SigV4
    C2 --> SigV4
    C3 --> SigV4
    SigV4 --> TLS
    TLS --> EC2Api
    TLS --> S3Api
    TLS --> IAMApi
```

### 1. AWS Management Console (GUI)
- **Nature**: Web-based graphical user interface accessible through standard modern web browsers.
- **Primary Strengths**: Intuitive visual dashboards, easy resource discovery, integrated cost exploration, and guided wizards for beginners.
- **Operational Limitations**:
  - Susceptible to human error ("fat-finger" configuration mistakes).
  - Cannot be automated or embedded into continuous deployment pipelines.
  - New service features or bleeding-edge API flags may not immediately have console graphical UI equivalents on day one of release.
  - Generates configuration drift across large-scale multi-account environments.

### 2. AWS Command Line Interface (AWS CLI)
- **Nature**: Unified, cross-platform binary executable executed directly from operating system command prompts (Linux terminal, macOS zsh/bash, Windows Command Prompt/PowerShell).
- **Primary Strengths**:
  - High operational velocity: Repeatable, deterministic, and easily chained with operating system text utilities (`grep`, `awk`, `jq`, `sed`).
  - Scriptable automation: Foundation of cron maintenance jobs, bootstrap scripts, and shell automation.
  - Complete API parity: Exposes virtually all AWS public API capabilities immediately upon service release.
- **Operational Limitations**: Requires command syntax familiarity, knowledge of parameter flags, and terminal access.

### 3. AWS Software Development Kits (SDKs)
- **Nature**: Language-specific software libraries (e.g., Python `boto3`, JavaScript/TypeScript `@aws-sdk`, Java SDK, Go SDK, .NET SDK).
- **Primary Strengths**:
  - Embedded directly inside custom application logic and microservices.
  - Automatically handles low-level complexities such as request retries, exponential backoff, connection pooling, and SigV4 request signing.
  - Provides strongly-typed data structures, object serialization, and native language exception handling.
- **Operational Limitations**: Requires software development expertise, compiler/interpreter toolchains, and code build pipelines.

### Detailed Comparison: AWS Access Interfaces

| Dimension | AWS Management Console | AWS Command Line Interface (CLI) | AWS Software Development Kits (SDKs) |
| :--- | :--- | :--- | :--- |
| **Interface Style** | Graphical User Interface (GUI) | Command-Line Interface (CLI) | Programmatic Code Library (API / SDK) |
| **Primary User** | Practitioners, Auditors, Architects | Systems Administrators, SysOps, DevOps | Software Developers, Application Architects |
| **Primary Use Case** | Visual inspection, ad-hoc discovery, learning | Operational automation, scripting, quick diagnostics | Application integration, complex custom systems |
| **Automation Suitability** | Very low (requires browser macros/Selenium) | **High** (shell scripts, cron, CI/CD runners) | **Very High** (custom software, event handlers) |
| **Learning Curve** | Gentle / Low | Moderate (syntax & parameter flags) | Steep (programming language dependency) |
| **Underlying Mechanism** | AWS REST API over HTTPS | AWS REST API over HTTPS | AWS REST API over HTTPS |
| **Authentication Form** | Username/Password + MFA | Access Key Pair / Temporary Session Token | IAM Roles / STS Tokens / Access Keys |
| **Day-1 API Parity** | Occasional lag for new capabilities | **Immediate** (available on API launch) | **Immediate** (supported across major SDKs) |

> [!NOTE]
> Regardless of which tool you use, AWS processes every request identically. An `aws ec2 run-instances` CLI command triggers the exact same backend API endpoint and IAM authorization evaluation as clicking **Launch Instance** in the AWS Console or invoking `ec2_client.run_instances()` in Python Boto3.

---

## 🛠️ 2. What is the AWS CLI & Version Evolution (v1 vs. v2)

### The Unified Tool Concept
Prior to the unified AWS CLI, administrators had to download disparate, dedicated command-line utilities for individual services (such as separate packages for EC2 tools and S3 tools). The AWS CLI unified these fragmented utilities into a single, cohesive executable (`aws`) that provides consistent syntax, unified configuration, and centralized authentication across all AWS services.

### AWS CLI Version 1 vs. AWS CLI Version 2
AWS CLI is distributed in two major release generations: **Version 1 (v1)** and **Version 2 (v2)**. Version 2 is the modern, recommended generation for all production and learning environments.

```mermaid
flowchart LR
    subgraph V1["AWS CLI Version 1 (Legacy)"]
        V1_Runtime["Python Dependency<br>(Requires host Python + pip)"]
        V1_Install["pip install awscli<br>(Dependency hell risks)"]
        V1_Features["Basic interactive prompts<br>Limited SSO integration"]
    end

    subgraph V2["AWS CLI Version 2 (Current Standard)"]
        V2_Runtime["Self-Contained Bundle<br>(Embedded isolated Python runtime)"]
        V2_Install["Native OS Installers<br>(RPM, PKG, Windows MSI, clean zip)"]
        V2_Features["• Auto-prompt (--cli-auto-prompt)<br>• Interactive wizards<br>• Native AWS SSO / IAM Identity Center<br>• Enhanced YAML output"]
    end

    V1 -.->|Evolved to| V2
    style V2 fill:#005A9C,stroke:#FF9900,stroke-width:2px,color:#FFFFFF
```

### Key Enhancements in AWS CLI Version 2

1. **Self-Contained Distribution Package**:
   - Version 1 was distributed as a Python package requiring an existing host Python installation and `pip`. This often caused Python version conflicts and dependency issues with other host software.
   - Version 2 is delivered as a **self-contained installer** with all runtime dependencies (including an isolated Python runtime) packaged internally. No local Python installation or package manager is required.
2. **Interactive Auto-Prompt Mode (`--cli-auto-prompt`)**:
   - Version 2 introduces an interactive auto-prompt feature. When enabled or passed on the command line, the CLI displays a responsive menu of services, subcommands, and available parameters with real-time documentation:
     ```bash
     aws --cli-auto-prompt
     ```
3. **Interactive Configuration Wizards**:
   - Expanded wizards simplify setting up complex environments, including configuring Amazon DynamoDB, AWS IAM Identity Center (formerly AWS SSO), and multi-account configurations:
     ```bash
     aws configure sso
     ```
4. **Native AWS IAM Identity Center (AWS SSO) Integration**:
   - Version 2 includes built-in support for browser-based Single Sign-On (SSO) login workflows, automatically managing temporary session credentials and token refreshing without third-party wrapper tools.
5. **Enhanced Output Formats**:
   - In addition to JSON, text, and table, v2 natively supports YAML formatted output for reading and parsing configuration structures.

### Comprehensive Comparison: Version 1 vs. Version 2

| Feature / Attribute | AWS CLI Version 1 | AWS CLI Version 2 |
| :--- | :--- | :--- |
| **Packaging & Delivery** | Python Package Index (`pip install awscli`) | Pre-compiled self-contained executable packages |
| **Python Dependency** | Requires system Python runtime & dependencies | **Zero external dependencies** (isolated runtime included) |
| **Update Mechanism** | `pip install --upgrade awscli` | Installer-based `--update` flag |
| **Auto-Prompt Capability** | Not available | Supported via `--cli-auto-prompt` |
| **AWS SSO / Identity Center** | Requires manual external tooling/plugins | **Native built-in command** (`aws configure sso`) |
| **Output Formats** | `json`, `text`, `table` | `json`, `text`, `table`, **`yaml`**, **`yaml-stream`** |
| **Recommended Usage** | Maintenance mode for legacy environments | **Default choice for all modern SysOps & DevOps** |

---

## 🐧 3. Installing, Verifying, & Updating the AWS CLI on Linux

The AWS CLI supports all major operating systems:
- **Linux**: x86_64 and ARM64 architecture bundles.
- **macOS**: GUI package installer (.pkg) or command line for Intel and Apple Silicon.
- **Microsoft Windows**: 64-bit MSI installer (`AWSCLIV2.msi`).

> [!NOTE]
> On **Amazon Linux 2** and **Amazon Linux 2023** AMIs, the AWS CLI comes pre-installed by default. However, verifying the installed version and knowing how to update to the latest release is a core SysOps competency.

### Linux Step-by-Step Installation Procedure

To install AWS CLI v2 on a 64-bit Linux distribution (such as Ubuntu, Debian, Red Hat Enterprise Linux, CentOS, or Amazon Linux), perform the following sequence:

```mermaid
flowchart TD
    Step1["1. Download Package<br>curl 'https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip' -o 'awscliv2.zip'"]
    Step2["2. Extract Archive<br>unzip awscliv2.zip"]
    Step3["3. Run Installer<br>sudo ./aws/install"]
    Step4["4. Verify Deployment<br>aws --version"]
    Step5["5. Clean Temporary Files<br>rm -rf awscliv2.zip aws"]

    Step1 --> Step2 --> Step3 --> Step4 --> Step5
```

#### Step 1: Download the Installation Package
Use the `curl` utility to fetch the official AWS CLI v2 zip archive. The `-o` flag specifies the output file name:
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

#### Step 2: Extract the Package
Use the `unzip` utility to unpack the archive. This creates an uncompressed directory named `aws` in your current working directory:
```bash
unzip awscliv2.zip
```

> [!TIP]
> If `unzip` is not installed on your Linux instance, install it using your distribution's package manager:
> - Debian/Ubuntu: `sudo apt-get update && sudo apt-get install -y unzip`
> - RHEL/Amazon Linux: `sudo yum install -y unzip`

#### Step 3: Execute the Installer
Run the `install` executable script with administrative privileges (`sudo`). By default, files are installed into `/usr/local/aws-cli`, and a symbolic link (`aws`) is created in `/usr/local/bin`:
```bash
sudo ./aws/install
```

If you wish to specify custom target paths, use the `--bin-dir` and `--install-dir` options:
```bash
sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli
```

#### Step 4: Verify the Installation
Verify that the `aws` binary is accessible on the system `$PATH` and inspect the version string:
```bash
aws --version
```

**Expected Command Output:**
```text
aws-cli/2.15.30 Python/3.11.8 Linux/5.10.205-195.807.amzn2.x86_64 exe/x86_64.amzn.2 prompt/off
```

**Deconstruction of the Version Output String:**
- `aws-cli/2.15.30`: The major, minor, and patch release version of the AWS CLI tool.
- `Python/3.11.8`: The internal Python runtime embedded inside the isolated CLI package.
- `Linux/5.10.205...`: The Linux operating system kernel version of the host.
- `exe/x86_64...`: The machine hardware architecture and distribution flavor.

#### Step 5: Clean Up Installation Artifacts
Remove the downloaded zip file and the extracted `aws` directory to free disk space:
```bash
rm -rf awscliv2.zip ./aws
```

### Updating the AWS CLI on Linux
To update an existing AWS CLI v2 installation to the latest available release, re-download and unzip the newest package, then invoke the installer with the `--update` parameter:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
rm -rf awscliv2.zip ./aws
aws --version
```

---

## ⚙️ 4. Configuring the AWS CLI: Credentials, Profiles, & Precedence

Once the AWS CLI is installed, it must be configured with identity credentials, regional targeting, and response formatting to communicate with AWS service endpoints.

### The Interactive Configuration Wizard: `aws configure`
The fastest method to configure basic CLI settings is the interactive command:
```bash
aws configure
```

The wizard prompts sequentially for four core configuration parameters:

```text
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-east-1
Default output format [None]: json
```

### Deconstructing the 4 Core Parameters

1. **AWS Access Key ID**:
   - A 20-character alphanumeric identifier (e.g., `AKIAIOSFODNN7EXAMPLE`) assigned to an IAM user.
   - Acts as the public username for programmatic API authentication.
2. **AWS Secret Access Key**:
   - A 40-character confidential cryptographic key (e.g., `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`).
   - Acts as the password used to calculate the HMAC-SHA256 signature for AWS Signature Version 4 (SigV4) request signing.
3. **Default Region Name**:
   - Specifies the geographic AWS Region where API calls will be directed if no `--region` flag is provided (e.g., `us-east-1` for N. Virginia, `us-west-2` for Oregon, `eu-west-1` for Ireland).
4. **Default Output Format**:
   - Specifies the default encoding format for command responses. Supported values: `json` (default), `text`, `table`, and `yaml`.

```mermaid
flowchart TD
    User["👨‍💻 SysOps Administrator"]
    Cmd["Runs: aws configure"]
    
    subgraph Files["~/.aws Directory Storage"]
        CredsFile["~/.aws/credentials<br>Permissions: chmod 600<br>--------------------------<br>[default]<br>aws_access_key_id = AKIA...<br>aws_secret_access_key = wJal..."]
        ConfigFile["~/.aws/config<br>Permissions: chmod 644<br>--------------------------<br>[default]<br>region = us-east-1<br>output = json"]
    end

    User --> Cmd
    Cmd -->|Writes sensitive keys| CredsFile
    Cmd -->|Writes operational preferences| ConfigFile
```

### The Configuration Storage Files: `~/.aws/`
The `aws configure` wizard creates and updates two plaintext files in the user's home directory under `~/.aws/`:

#### 1. Credentials File: `~/.aws/credentials`
Stores sensitive authentication secrets. It is strictly segregated so that read/write access permissions can be locked down:
```ini
[default]
aws_access_key_id = AKIAIOSFODNN7EXAMPLE
aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

[production]
aws_access_key_id = AKIAI44QH8DHBEXAMPLE
aws_secret_access_key = je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
```

> [!CAUTION]
> Protect `~/.aws/credentials` using strict Linux file permissions. Run `chmod 600 ~/.aws/credentials` to ensure that only the owning user can read or write the file.

#### 2. Configuration File: `~/.aws/config`
Stores non-sensitive operational settings such as regions, output formats, retry modes, and role assumption profiles:
```ini
[default]
region = us-east-1
output = json

[profile production]
region = us-west-2
output = table
```

> [!NOTE]
> Notice the section header syntax difference:
> - In `~/.aws/credentials`, named profiles are declared as `[profile_name]`.
> - In `~/.aws/config`, named profiles are declared as `[profile profile_name]` (except for `[default]`).

### Working with Named Profiles
SysOps engineers frequently manage multiple AWS accounts (e.g., Development, Staging, Production) or operate under distinct IAM roles. **Named profiles** allow switching contexts effortlessly.

To configure a named profile:
```bash
aws configure --profile production
```

To execute a command using a specific profile:
```bash
aws s3 ls --profile production
```

Alternatively, set the `AWS_PROFILE` environment variable in your current shell session to avoid passing `--profile` on every command:
```bash
export AWS_PROFILE=production
aws ec2 describe-instances
```

### Configuration and Credential Precedence Hierarchy
When executing a command, the AWS CLI searches multiple locations for credentials and configuration settings in a strict priority order. The first value found takes precedence:

```mermaid
flowchart TD
    P1["1. Command-Line Options<br>(--region, --output, --profile)"]
    P2["2. Environment Variables<br>(AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_DEFAULT_REGION)"]
    P3["3. Assume Role / Web Identity<br>(Configured in ~/.aws/config)"]
    P4["4. AWS Credentials File<br>(~/.aws/credentials)"]
    P5["5. AWS Config File<br>(~/.aws/config)"]
    P6["6. ECS Task Role / EC2 Instance Profile<br>(IAM Role attached to container or EC2 instance metadata)"]

    P1 -->|If not set| P2
    P2 -->|If not set| P3
    P3 -->|If not set| P4
    P4 -->|If not set| P5
    P5 -->|If not set| P6
```

---

## 🧩 5. AWS CLI Command Structure & Syntax Anatomy

The AWS CLI enforces a consistent, intuitive command anatomy across all supported services:

```text
aws <service> <operation> [parameters/arguments] [options]
```

```mermaid
flowchart LR
    Executable["aws"] --> Service["ec2"]
    Service --> Subcommand["stop-instances"]
    Subcommand --> Parameters["--instance-ids i-1234567890abcdef0"]
    Parameters --> Options["--output json --region us-east-1"]

    style Executable fill:#232F3E,color:#FFFFFF
    style Service fill:#FF9900,color:#000000
    style Subcommand fill:#005A9C,color:#FFFFFF
    style Parameters fill:#4A90E2,color:#FFFFFF
    style Options fill:#7ED321,color:#000000
```

### Deconstruction of Command Parts

1. **Executable (`aws`)**:
   - Calls the AWS CLI binary executable located on the system `$PATH`.
2. **Service (`<service>`)**:
   - Specifies the target AWS service (e.g., `ec2`, `s3`, `iam`, `rds`, `vpc`, `lambda`, `cloudwatch`).
3. **Operation / Subcommand (`<operation>`)**:
   - Specifies the action or API call to perform against the service (e.g., `run-instances`, `describe-instances`, `create-bucket`, `put-metric-data`).
4. **Parameters / Arguments (`[parameters]`)**:
   - Inputs required or accepted by the specific operation. Preceded by double dashes (`--`).
   - Example: `--instance-ids i-0123456789abcdef0`, `--image-id ami-0c55b159cbfafe1f0`.
5. **Options / Global Parameters (`[options]`)**:
   - Flags available across all services to alter output format, regional routing, authentication, or debugging.
   - Example: `--output json`, `--region us-west-2`, `--profile staging`, `--dry-run`.

### Concrete Command Syntax Examples

#### Example 1: Stopping an EC2 Instance
```bash
aws ec2 stop-instances --instance-ids i-1234567890abcdef0 --output json
```
- **Service**: `ec2`
- **Operation**: `stop-instances`
- **Parameter**: `--instance-ids i-1234567890abcdef0`
- **Option**: `--output json`

#### Example 2: Launching an Instance Using JSON Input File
Complex commands with numerous configuration parameters can quickly become unwieldy on a single terminal line. The AWS CLI supports passing an entire JSON input document using the `file://` protocol prefix:
```bash
aws ec2 run-instances --cli-input-json file://webserver.json
```
- **Parameter**: `--cli-input-json file://webserver.json` loads all launch parameters (AMI ID, instance type, security group IDs, subnet ID, tags) from a local configuration file named `webserver.json`.

> [!TIP]
> You can automatically generate a complete JSON template for any command using the `--generate-cli-skeleton` flag:
> ```bash
> aws ec2 run-instances --generate-cli-skeleton > template.json
> ```

---

## 📖 6. Built-in Interactive Help System

The AWS CLI contains comprehensive, built-in documentation derived directly from the official AWS API reference manuals. Systems operators can access instant syntax documentation, parameter definitions, and usage examples without leaving the terminal shell.

The help system operates hierarchically across three contextual levels:

```mermaid
flowchart TD
    H1["Level 1: Global Help<br>aws help<br>Lists all available services and global flags"]
    H2["Level 2: Service Help<br>aws ec2 help<br>Lists service description and all subcommands"]
    H3["Level 3: Operation Help<br>aws ec2 describe-instances help<br>Detailed parameters, options, and JSON examples"]

    H1 --> H2 --> H3
```

### 1. Global Help (`aws help`)
Displays all global CLI flags and an exhaustive list of every supported AWS service:
```bash
aws help
```

### 2. Service-Level Help (`aws <service> help`)
Displays a comprehensive description of the specific service and enumerates all available subcommands and operations:
```bash
aws ec2 help
```
*Output snippet:*
```text
NAME
       ec2 -

DESCRIPTION
       Amazon Elastic Compute Cloud (Amazon EC2) provides secure and
       resizable computing capacity in the AWS Cloud...

AVAILABLE COMMANDS
       o attach-volume
       o copy-snapshot
       o create-image
       o create-volume
       o delete-snapshot
       o describe-instances
       o run-instances
       o start-instances
       o stop-instances
```

### 3. Operation-Level Help (`aws <service> <operation> help`)
Provides exhaustive technical documentation for a specific API operation, including required parameters, valid argument types, return structures, and practical copy-paste CLI examples:
```bash
aws ec2 describe-instances help
```

> [!NOTE]
> The help system utilizes your terminal's default pager (typically `less` or `more`). Navigate using the Arrow keys or `Page Up`/`Page Down`, search using `/search_term`, and press `q` to exit back to your prompt.

---

## 📊 7. Output Formatting, JMESPath Filtering, & Server-Side Filtering

### Understanding Output Formats

The AWS CLI supports four primary output formats. You can set the default format globally via `aws configure` or override it per command using the `--output` option or the `AWS_DEFAULT_OUTPUT` environment variable.

| Output Format | Structural Representation | Primary Use Case & Characteristics |
| :--- | :--- | :--- |
| **`json`** (Default) | JavaScript Object Notation | Standard for automated scripting, CI/CD pipelines, and parsing via tools like `jq`. Preserves nested object hierarchies. |
| **`table`** | ASCII Formatted Tables | Optimal for human reading and rapid manual inspection in interactive terminal sessions. Converts arrays into columnar grids. |
| **`text`** | Tab-delimited lines | Ideal for classic Unix stream processing utilities (`awk`, `cut`, `grep`, `sed`, `sort`, `xargs`). |
| **`yaml`** (v2 only) | YAML Ain't Markup Language | Highly readable hierarchical format, useful for generating configuration manifests. |

#### Demonstration of Output Format Differences

##### JSON Output (Default):
```bash
aws ec2 describe-instances --instance-ids i-068035fce1e9abcc9 --output json
```
```json
{
    "Reservations": [
        {
            "Instances": [
                {
                    "InstanceId": "i-068035fce1e9abcc9",
                    "InstanceType": "m5.large",
                    "State": {
                        "Code": 16,
                        "Name": "running"
                    },
                    "PrivateIpAddress": "10.0.50.14"
                }
            ]
        }
    ]
}
```

##### Table Output:
```bash
aws ec2 describe-instances --instance-ids i-068035fce1e9abcc9 --output table
```
```text
---------------------------------------------------------------------------------
|                               DescribeInstances                               |
+-------------------------------------------------------------------------------+
||                                Reservations                                 ||
|+-----------------------------------------------------------------------------+|
|||                                 Instances                                 |||
|+---------------------+--------------+----------------------------------------+|
|||  InstanceId        | InstanceType | PrivateIpAddress                       |||
|+---------------------+--------------+----------------------------------------+|
|||  i-068035fce1e9abcc9| m5.large     | 10.0.50.14                             |||
|+---------------------+--------------+----------------------------------------+|
```

##### Text Output:
```bash
aws ec2 describe-instances --instance-ids i-068035fce1e9abcc9 --output text
```
```text
RESERVATIONS    r-c242341e      230357233174
INSTANCES       ami-423bec20    i-068035fce1e9abcc9     m5.large        running 10.0.50.14
```

---

### Server-Side Filtering (`--filter` / `--filters`)
**Server-side filtering** evaluates filter criteria directly on the AWS API endpoint servers *before* data is serialized and transmitted across the network to your local terminal.

- **Mechanism**: The query parameters are passed in the HTTP request payload; AWS evaluates matching resources in the cloud and sends back only the matching subset.
- **Benefits**: Drastically reduces network bandwidth, decreases API latency, and eliminates client-side memory overload when querying large resource pools (e.g., thousands of EC2 instances or EBS volumes).

```bash
# Filter for only Windows instances
aws ec2 describe-instances --filter "Name=platform,Values=windows"

# Filter for instances that are currently running
aws ec2 describe-instances --filter "Name=instance-state-name,Values=running"

# Filter using multiple values for a key
aws ec2 describe-instances --filter "Name=instance-type,Values=t2.micro,t2.small"
```

---

### Client-Side Querying (`--query` with JMESPath)
**Client-side querying** uses the `--query` flag to process and extract specific fields from the JSON payload *after* the complete response has been received by the local AWS CLI client.

The AWS CLI implements the **JMESPath** specification (a JSON query language).

```mermaid
flowchart LR
    API["AWS Cloud API Endpoint"] -->|Full JSON Payload sent over network| Client["AWS CLI Client on Terminal"]
    Client -->|Applies JMESPath --query| Display["Terminal Screen Filtered Output"]
```

#### JMESPath Syntax Rules & Patterns

1. **Zero-Based Array Indexing**:
   - Access specific elements in a list: `Reservations[0].Instances[0]` extracts the first instance of the first reservation.
2. **Field Chaining**:
   - Access nested object properties using dot notation: `Reservations[0].Instances[0].State.Name` extracts only the string value `"running"` or `"terminated"`.
3. **Array Wildcards (`[*]`)**:
   - Flatten and project attributes across every member of an array: `Reservations[*].Instances[*].InstanceId`.
4. **Multi-Field Projections**:
   - Return multiple specific attributes formatted as a list: `Reservations[].Instances[][InstanceId, State.Name, PrivateIpAddress]`.

#### Step-by-Step JMESPath Query Progression

```bash
# 1. Query the first instance of the first reservation (returns partial JSON block)
aws ec2 describe-instances --query 'Reservations[0].Instances[0]'

# 2. Query a specific deeply nested string value (returns a single word: "running")
aws ec2 describe-instances --query 'Reservations[0].Instances[0].State.Name'

# 3. Query all instance states across all reservations using wildcards
aws ec2 describe-instances --query 'Reservations[*].Instances[*].State.Name'

# 4. Extract all instance IDs in a clean text list for a shell loop
aws ec2 describe-instances --query "Reservations[*].Instances[*].InstanceId" --output text
```

---

### Combining Server-Side `--filter` and Client-Side `--query`
For peak operational efficiency, SysOps administrators combine both mechanisms:
- Use `--filter` to instruct AWS servers to return only the subset of resources matching criteria.
- Use `--query` to strip away unnecessary JSON metadata and output only the exact fields required.

```bash
aws ec2 describe-instances \
    --filter "Name=instance-type,Values=t2.micro,t2.small" \
    --query "Reservations[*].Instances[*].InstanceId" \
    --output text
```

### Architectural Comparison: `--filter` vs. `--query`

| Criterion | Server-Side `--filter` | Client-Side `--query` |
| :--- | :--- | :--- |
| **Execution Location** | AWS Cloud Endpoint Servers | Local Administrator Machine |
| **Network Data Transferred** | Only matching records sent over wire | Full JSON response transmitted over wire |
| **Query Engine** | Service-specific API filter parameters | Standard JMESPath query engine |
| **Output Transformation** | Cannot alter JSON schema structure | Can reshape, project, and flatten output |
| **Availability** | Dependent on service API implementation | Available on **every** AWS CLI command |

---

## 🧪 8. The `--dry-run` Option & Defensive Execution

When managing production environments, executing modifying commands (such as launching instances, terminating servers, or updating security groups) carries operational risk.

The `--dry-run` parameter allows systems administrators to **test whether a command has the required IAM permissions and valid syntax without actually executing the action or modifying cloud state**.

```mermaid
flowchart TD
    RunCmd["aws ec2 run-instances ... --dry-run"] --> IAMCheck{"IAM Permission & Syntax Check"}
    
    IAMCheck -->|Authorized & Valid| DryRunSuccess["Exit with Code / DryRunOperation<br>'Request would have succeeded, but DryRun flag is set.'"]
    IAMCheck -->|Missing IAM Permission| AuthError["Exit with Error / UnauthorizedOperation<br>'You are not authorized to perform this operation.'"]
    IAMCheck -->|Invalid Parameter| ParamError["Exit with Error / InvalidParameterValue<br>Reports syntax or missing required argument"]

    style DryRunSuccess fill:#7ED321,color:#000000
    style AuthError fill:#D0021B,color:#FFFFFF
    style ParamError fill:#FF9900,color:#000000
```

### Dry-Run Example: Launching an EC2 Instance
```bash
aws ec2 run-instances \
    --image-id ami-1a2b3c4d \
    --count 1 \
    --instance-type c5.large \
    --key-name MyKeyPair \
    --security-groups MySecurityGroup \
    --dry-run
```

### Possible Dry-Run Responses

#### Scenario A: Authorized and Valid
If the operator has valid IAM permissions to run the operation and all parameters are properly structured, AWS intercepts the request before resource creation and returns:
```text
An error occurred (DryRunOperation) when calling the RunInstances operation: 
Request would have succeeded, but DryRun flag is set.
```
> [!TIP]
> Notice that AWS returns this as an exception named `DryRunOperation`. In bash automation scripts, checking for `DryRunOperation` confirms that credentials and permissions are fully validated.

#### Scenario B: Missing Permissions
If the operator lacks permission in their IAM policy (e.g., missing `ec2:RunInstances`), AWS immediately rejects the request:
```text
An error occurred (UnauthorizedOperation) when calling the RunInstances operation: 
You are not authorized to perform this operation.
```

---

## 🚀 9. Common Operational Commands: Amazon EC2 & Amazon S3

### Essential Amazon EC2 Commands

The `aws ec2` command family manages virtual servers, networking (VPCs, subnets, route tables), and storage volumes (EBS).

```bash
# 1. Describe all EC2 instances in the region
aws ec2 describe-instances

# 2. Launch a new EC2 instance from an Amazon Machine Image (AMI)
aws ec2 run-instances \
    --image-id ami-0c55b159cbfafe1f0 \
    --instance-type t3.micro \
    --key-name ProductionKey \
    --subnet-id subnet-0123456789abcdef0

# 3. Stop an active EC2 instance cleanly
aws ec2 stop-instances --instance-ids i-0123456789abcdef0

# 4. Start a stopped EC2 instance
aws ec2 start-instances --instance-ids i-0123456789abcdef0

# 5. Create an Elastic Block Store (EBS) volume (100 GiB gp3 in us-east-1a)
aws ec2 create-volume \
    --availability-zone us-east-1a \
    --size 100 \
    --volume-type gp3

# 6. Create a Virtual Private Cloud (VPC) with a designated CIDR block
aws ec2 create-vpc --cidr-block 10.0.0.0/16
```

---

### Essential Amazon S3 Commands

AWS provides two levels of S3 CLI commands:
- **`aws s3` (High-Level)**: Simplifies common object manipulation routines (copy, move, sync, remove, list) with file-system-like syntax.
- **`aws s3api` (Low-Level)**: Directly maps 1:1 to every raw S3 REST API call (e.g., `put-bucket-cors`, `get-bucket-policy`).

```bash
# 1. List all S3 buckets in the AWS account
aws s3 ls

# 2. List all objects and common prefixes inside a specific bucket
aws s3 ls s3://my-operational-backup-bucket/

# 3. Copy a local file to an S3 bucket
aws s3 cp /var/log/syslog s3://my-operational-backup-bucket/logs/syslog

# 4. Copy an S3 object down to the local file system
aws s3 cp s3://my-operational-backup-bucket/configs/app.conf ./app.conf

# 5. Copy an object directly between two S3 buckets (cloud-to-cloud transfer)
aws s3 cp s3://source-bucket/data.csv s3://destination-bucket/data.csv

# 6. Move an object (copies to target and deletes from source)
aws s3 mv ./local-report.pdf s3://my-operational-backup-bucket/reports/

# 7. Delete an individual S3 object
aws s3 rm s3://my-operational-backup-bucket/temp-file.tmp

# 8. Recursively delete an entire S3 bucket directory tree
aws s3 rm s3://my-operational-backup-bucket/old-data/ --recursive

# 9. Synchronize local directory with S3 bucket (only uploads modified/new files)
aws s3 sync /var/www/html/ s3://my-website-bucket/assets/ --delete
```

---

## 🔒 10. Programmatic Security Best Practices for Systems Operations

Access keys (`AKIA...` and secret pairs) provide long-term programmatic administrative access. Compromised access keys are a primary vector for cloud security breaches and cryptocurrency mining exploits.

```mermaid
flowchart TD
    subgraph AntiPatterns["❌ High-Risk Anti-Patterns"]
        AP1["Hardcoding access keys in bash scripts"]
        AP2["Committing credentials to Git / GitHub"]
        AP3["Using Root user access keys for daily work"]
        AP4["Sharing long-lived access keys across team members"]
    end

    subgraph BestPractices["✅ Production Security Best Practices"]
        BP1["Use IAM Roles & EC2 Instance Profiles"]
        BP2["Enforce Least Privilege IAM Policies"]
        BP3["Leverage AWS IAM Identity Center (SSO)"]
        BP4["Rotate Access Keys Regularly (90 days)"]
        BP5["Utilize git-secrets & .gitignore"]
    end

    AntiPatterns -.->|Replace with| BestPractices
```

### 1. Eliminate Long-Lived Access Keys on Compute Resources
**Never store access keys in scripts running on EC2 instances, ECS containers, or Lambda functions.** Instead, attach an **IAM Role via an EC2 Instance Profile**:
- The AWS CLI automatically queries the **Instance Metadata Service (IMDSv2)** at `http://169.254.169.254/latest/meta-data/iam/security-credentials/`.
- The AWS Security Token Service (STS) provides temporary, automatically rotated credentials.
- No secrets are stored on disk.

### 2. Never Commit Access Keys to Version Control
- Add `.aws/` and sensitive configuration files to `.gitignore`.
- Implement client-side pre-commit hooks such as `git-secrets` or automated scanners (e.g., TruffleHog, GitHub Secret Scanning) to prevent accidental credential commits.
- If an access key is inadvertently committed to a public repository, consider it instantly compromised. Immediately deactivate and delete it via the IAM console.

### 3. Enforce the Principle of Least Privilege
- Never grant `AdministratorAccess` or wildcard `*` permissions to operational scripts or CI/CD pipelines.
- Scope IAM policies down to specific actions and target Resource Amazon Resource Names (ARNs):
  ```json
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "s3:PutObject",
                  "s3:GetObject"
              ],
              "Resource": "arn:aws:s3:::cafe-daily-backups/*"
          }
      ]
  }
  ```

### 4. Understand Environment Variable Precedence in CI/CD Pipelines
In automated deployment runners (e.g., GitHub Actions, GitLab CI, AWS CodeBuild), avoid writing credentials to disk. Instead, inject them as protected environment variables:
```bash
export AWS_ACCESS_KEY_ID="ASIATEMPEXAMPLEKEY"
export AWS_SECRET_ACCESS_KEY="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
export AWS_SESSION_TOKEN="AQoDYXdzEJr1...EXAMPLETOKEN=="
export AWS_DEFAULT_REGION="us-east-1"
```

---

## ☕ 11. Café & Bakery Real-World Application Scenario

### The Operational Challenge
At the **Café and Bakery**, Frank and Martha are scaling their digital ordering platform. Sofía (the cloud engineer) and Nikhil (the systems administrator) manage multiple EC2 web servers hosting the bakery order portal and an S3 bucket storing product photos, daily receipts, and menu assets.

Manual console administration creates bottlenecks:
1. Every evening, Nikhil manually inspects whether web instances in different Availability Zones are running.
2. Frank needs static menu PDFs and marketing graphics updated across web servers without manually logging into the console.
3. System backups must run predictably at midnight.

### Automated Solution: SysOps Automation Bash Script

Sofía implements a production bash script leveraging the AWS CLI, server-side filtering, and JMESPath querying:

```bash
#!/usr/bin/env bash
# ==============================================================================
# Cafe & Bakery: Automated Instance Health Check & Backup Script
# Author: Sofia, Systems Operations Engineer
# ==============================================================================
set -euo pipefail

BACKUP_BUCKET="s3://cafe-bakery-backups-us-east-1"
APP_DIR="/var/www/cafe/media"
REGION="us-east-1"

echo "=== [1/3] Checking Running EC2 Web Servers ==="
# Query instance IDs and private IP addresses for running web servers using filter & query
aws ec2 describe-instances \
    --region "${REGION}" \
    --filter "Name=tag:Role,Values=WebServer" "Name=instance-state-name,Values=running" \
    --query "Reservations[].Instances[][InstanceId, PrivateIpAddress, Placement.AvailabilityZone]" \
    --output table

echo "=== [2/3] Synchronizing Cafe Media Assets to Amazon S3 ==="
# Sync menu graphics and receipts to S3 (upload only changed files)
aws s3 sync "${APP_DIR}" "${BACKUP_BUCKET}/media-assets/" \
    --region "${REGION}" \
    --delete

echo "=== [3/3] Creating Nightly EBS Snapshot of Order Database ==="
# Locate the database volume ID
DB_VOL_ID=$(aws ec2 describe-volumes \
    --region "${REGION}" \
    --filter "Name=tag:Application,Values=OrderDB" \
    --query "Volumes[0].VolumeId" \
    --output text)

if [[ "${DB_VOL_ID}" != "None" && -n "${DB_VOL_ID}" ]]; then
    SNAPSHOT_ID=$(aws ec2 create-snapshot \
        --volume-id "${DB_VOL_ID}" \
        --description "Nightly automated snapshot of OrderDB" \
        --tag-specifications 'ResourceType=snapshot,Tags=[{Key=Environment,Value=Production}]' \
        --query "SnapshotId" \
        --output text)
    echo "Created snapshot: ${SNAPSHOT_ID} for volume: ${DB_VOL_ID}"
else
    echo "Error: Database volume not found!"
    exit 1
fi

echo "=== All Cafe SysOps maintenance routines completed successfully! ==="
```

---

## ❓ 12. Knowledge Check & Checkpoint Review

### Slide Checkpoint Questions & Detailed Answers

#### Question 1: Which operating systems can the AWS CLI be installed on?
- **Answer**: The AWS CLI is fully cross-platform and can be installed on:
  - **Linux** (x86_64 and ARM64 distributions, including Amazon Linux, Ubuntu, Debian, RHEL, CentOS)
  - **macOS** (Intel and Apple Silicon / M-series architectures)
  - **Microsoft Windows** (64-bit systems via MSI installer)
  - It can also be deployed inside containerized environments (Docker, Podman) and compute workloads (Amazon EC2 instances).

#### Question 2: How do system operators check which version of the AWS CLI they are using in Linux?
- **Answer**: Run the command:
  ```bash
  aws --version
  ```
  This command displays the installed version of the AWS CLI executable, the packaged internal Python runtime, the host operating system kernel, and underlying library components (such as botocore).

#### Question 3: What is the purpose of the `--dry-run` option in an AWS CLI command?
- **Answer**: The `--dry-run` option checks whether the requesting identity possesses the required IAM authorization permissions and verifies that parameter syntax is valid **without actually making the request or modifying cloud state**.
  - If permissions and parameters are valid: Returns `DryRunOperation` ("Request would have succeeded, but DryRun flag is set").
  - If permissions are missing: Returns `UnauthorizedOperation` ("You are not authorized to perform this operation").

---

### Extended Conceptual Review Questions

#### Question 4: What is the fundamental difference between the `--filter` option and the `--query` option?
- **Answer**:
  - `--filter` is evaluated **server-side** by AWS before returning data. It limits the volume of data sent across the network, reducing latency and bandwidth.
  - `--query` is evaluated **client-side** by the local AWS CLI using the JMESPath engine. It parses the returned JSON payload to extract, project, and reshape specific fields for display or shell scripts.

#### Question 5: When `aws configure` is executed, where are the Access Key ID and Secret Access Key stored, and what security precaution should be applied?
- **Answer**: They are stored in plaintext in the file `~/.aws/credentials` under the `[default]` profile header. The file should be protected using strict Linux file system permissions (`chmod 600 ~/.aws/credentials`) so that only the owning user account can read or write to it.

#### Question 6: Why should production EC2 instances use IAM Roles rather than stored AWS Access Keys?
- **Answer**: IAM Roles eliminate the need to store long-lived, hardcoded credentials on the instance's file system. Credentials are automatically issued, provided via the Instance Metadata Service (IMDS), and rotated regularly by AWS Security Token Service (STS). If an instance is compromised, attackers cannot obtain permanent credentials.

---

## 🔑 13. Key Takeaways & Operational Summary

- **Unified Control Plane**: The AWS CLI is a unified, cross-platform command-line tool that controls all AWS services through consistent syntax and unified authentication.
- **REST API Foundation**: The Management Console, AWS CLI, and SDKs are all interfaces that generate signed HTTP/HTTPS requests to underlying AWS REST API endpoints using AWS Signature Version 4 (SigV4).
- **AWS CLI v2 Advantages**: Version 2 packages a self-contained runtime with zero external Python dependencies, native AWS SSO integration, `--cli-auto-prompt`, and enhanced YAML output.
- **Installation Workflow**: On Linux, install by downloading the zip package via `curl`, extracting with `unzip`, executing `sudo ./aws/install`, and verifying with `aws --version`. Updates use `sudo ./aws/install ... --update`.
- **Configuration Quartet**: The `aws configure` wizard configures four core parameters: Access Key ID, Secret Access Key, Default Region Name, and Default Output Format, written to `~/.aws/credentials` and `~/.aws/config`.
- **Command Anatomy**: Every command follows `aws <service> <operation> [parameters] [options]`.
- **Dual-Layer Filtering**: Combine server-side `--filter` (reduces network traffic) with client-side `--query` (JMESPath extraction) to craft high-efficiency automation scripts.
- **Defensive Execution**: Use `--dry-run` to validate IAM permissions and parameter syntax before executing state-altering commands.
- **Credential Hygiene**: Never hardcode access keys in scripts or git repositories. Prioritize IAM Roles and EC2 Instance Profiles for compute workloads.
