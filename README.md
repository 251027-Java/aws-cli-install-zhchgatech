[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/okjAEuoW)
# Lab: Installing AWS CLI and EB CLI

## Goal

Install and configure the AWS Command Line Interface (AWS CLI) and Elastic Beanstalk CLI (EB CLI) tools for deploying applications to AWS.

## Learning Objectives

- Install the AWS CLI on your operating system
- Configure AWS credentials for CLI access
- Install the Elastic Beanstalk CLI using pip
- Verify both CLI tools are working correctly
- Understand the difference between AWS CLI and EB CLI

## Pre-requisites

- An AWS account (free tier is sufficient)
- Python 3.8+ installed (for EB CLI)
- Administrator access to your computer

## Background

The **AWS CLI** is a unified tool to manage AWS services from the command line. It allows you to control multiple AWS services, automate tasks through scripts, and is essential for DevOps workflows.

The **EB CLI** is specifically designed for Elastic Beanstalk, providing commands to create, configure, and deploy applications to AWS Elastic Beanstalk environments.

---

## Tasks

### Task 1: Install AWS CLI

#### Windows

1. Download the AWS CLI MSI installer:

   ```powershell
   # Run in PowerShell as Administrator
   msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
   ```

2. Or download manually from: <https://aws.amazon.com/cli/>

3. **Restart your terminal** after installation.

4. Verify installation:

   ```bash
   aws --version
   # Expected: aws-cli/2.x.x Python/3.x.x Windows/10 ...
   ```

#### macOS

```bash
# Using Homebrew (recommended)
brew install awscli

# Verify installation
aws --version
```

#### Linux

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

---

### Task 2: Configure AWS Credentials

1. Log in to the AWS Console and navigate to **IAM → Users → Your User → Security Credentials**

2. Create an Access Key (if you don't have one):
   - Click "Create access key"
   - Select "Command Line Interface (CLI)"
   - Download the credentials (you won't see them again!)

3. Run the configuration command:

   ```bash
   aws configure
   ```

4. Enter your credentials when prompted:

   ```
   AWS Access Key ID [None]: YOUR_ACCESS_KEY_ID
   AWS Secret Access Key [None]: YOUR_SECRET_ACCESS_KEY
   Default region name [None]: us-east-1
   Default output format [None]: json
   ```

5. Verify your configuration:

   ```bash
   aws sts get-caller-identity
   ```

   Expected output:

   ```json
   {
       "UserId": "AIDAEXAMPLE12345",
       "Account": "123456789012",
       "Arn": "arn:aws:iam::123456789012:user/your-username"
   }
   ```

> ⚠️ **Security Warning**: Never share or commit your AWS credentials. They provide full access to your AWS account!

---

### Task 3: Install EB CLI

The EB CLI is installed using Python's pip package manager.

1. Verify Python is installed:

   ```bash
   python --version
   # Expected: Python 3.8 or higher
   ```

2. Install EB CLI:

   ```bash
   pip install awsebcli --upgrade
   ```

3. If you get a permission error:

   ```bash
   pip install --user awsebcli --upgrade
   ```

4. **Add to PATH** (if `eb` command is not found):

   First, find the install location:

   ```bash
   pip show awsebcli
   # Look for the "Location" line
   ```

   The `eb` executable is in the Python Scripts folder. On Windows, this is typically:

   ```
   C:\Users\YourName\AppData\Roaming\Python\Python3xx\Scripts
   ```

   Add this to your system PATH (see Hints below).

5. **Restart your terminal**, then verify:

   ```bash
   eb --version
   # Expected: EB CLI 3.x.x (Python 3.x.x)
   ```

---

### Task 4: Test Both CLIs

1. Test AWS CLI by listing S3 buckets:

   ```bash
   aws s3 ls
   # Shows your S3 buckets (may be empty)
   ```

2. Test EB CLI by checking available platforms:

   ```bash
   eb platform list
   # Shows available Elastic Beanstalk platforms
   ```

---

## Hints

<details>
<summary>Hint 1: Adding to PATH on Windows</summary>

**Via GUI:**

1. Press `Win + R`, type `sysdm.cpl`, press Enter
2. Click **Advanced** tab → **Environment Variables**
3. Under "User variables", select **Path** → Click **Edit**
4. Click **New** → Paste the Scripts folder path
5. Click **OK** on all dialogs
6. Restart your terminal

**Via PowerShell (as Administrator):**

```powershell
$scriptsPath = "C:\Users\YourName\AppData\Roaming\Python\Python3xx\Scripts"
$currentPath = [Environment]::GetEnvironmentVariable("Path", "User")
[Environment]::SetEnvironmentVariable("Path", "$currentPath;$scriptsPath", "User")
```

</details>

<details>
<summary>Hint 2: Common EB CLI Installation Issues</summary>

| Issue | Solution |
|-------|----------|
| `'eb' is not recognized` | Add Python Scripts folder to PATH |
| Permission errors | Use `pip install --user awsebcli` |
| Python not found | Install Python from python.org |
| pip not found | Run `python -m pip install awsebcli` |

</details>

<details>
<summary>Hint 3: Alternative Installation with pipx</summary>

pipx automatically handles PATH for CLI tools:

```bash
pip install pipx
pipx install awsebcli
eb --version
```

</details>

---

## Expected Output

### AWS CLI Version Check

```
$ aws --version
aws-cli/2.15.0 Python/3.11.6 Windows/10 exe/AMD64 prompt/off
```

### AWS Identity Check

```
$ aws sts get-caller-identity
{
    "UserId": "AIDAEXAMPLE12345",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/trainee"
}
```

### EB CLI Version Check

```
$ eb --version
EB CLI 3.25.3 (Python 3.12.0)
```

---

## Deliverables

1. Screenshot showing `aws --version` output
2. Screenshot showing `aws sts get-caller-identity` output
3. Screenshot showing `eb --version` output

---

## Stretch Goals

- Create a named AWS profile for a different region
- Explore `aws configure list` to see all configured settings
- Run `eb init` in an empty folder to see the interactive setup

---

## Resources

- [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [EB CLI Installation Guide](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html)
- [AWS CLI Configuration](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
