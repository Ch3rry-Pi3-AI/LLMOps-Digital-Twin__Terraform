# 🛠️ **Install Terraform**

This branch focuses on installing Terraform correctly across macOS, Linux, and Windows, following the updated (2025) HashiCorp licensing and distribution changes.

Follow the appropriate set of instructions below for your operating system, then verify the installation.

## macOS (Homebrew Installation)

The easiest and most reliable way to install Terraform on macOS is via Homebrew:

```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

This installs the official HashiCorp-managed version and keeps it updated through the standard brew workflow.

## macOS / Linux (Manual Installation)

If you prefer manual installation or you're on a Linux distribution:

1. Visit the official install page:
   [https://developer.hashicorp.com/terraform/install](https://developer.hashicorp.com/terraform/install)

2. Download the correct package for your system.

3. Extract the archive and move the binary into your PATH.

Example for macOS (adjust the URL for your OS and architecture):

```bash
curl -O https://releases.hashicorp.com/terraform/1.10.0/terraform_1.10.0_darwin_amd64.zip
unzip terraform_1.10.0_darwin_amd64.zip
sudo mv terraform /usr/local/bin/
```

After moving the binary, Terraform becomes globally accessible.

## Windows Installation

1. Visit the official install page:
   [https://developer.hashicorp.com/terraform/install](https://developer.hashicorp.com/terraform/install)

2. Download the Windows package (`terraform_x.x.x_windows_amd64.zip`)

3. Extract the archive — inside you will find **terraform.exe**

4. Move `terraform.exe` to a permanent folder, for example:

```
C:\Program Files\Terraform\
```

5. Add this folder to your PATH:

   * Right-click **This PC**
   * **Properties**
   * **Advanced system settings**
   * **Environment Variables**
   * Under **System variables**, edit **Path**
   * Add the folder:

```
C:\Program Files\Terraform\
```

6. Save and open a new PowerShell window.

Terraform does not have a graphical interface — you will not see a program window when you double-click it. It only works through the terminal.

## Verify Installation

After installation, open a **new terminal session** and run:

```bash
terraform --version
```

A correct installation will show something like:

```
Terraform v1.14.0
```

(Versions will vary depending on what you installed.)

## Update Your `.gitignore`

Add Terraform-related ignore entries to prevent local state files, lock files, and sensitive information from being committed:

```gitignore
# Terraform
*.tfstate
*.tfstate.*
.terraform/
.terraform.lock.hcl
terraform.tfstate.d/
*.tfvars
!terraform.tfvars
!prod.tfvars

# Lambda packages
lambda-deployment.zip
lambda-package/

# Environment files
.env
.env.*

# Node
node_modules/
out/
.next/

# Python
__pycache__/
*.pyc
.venv/
uv.lock

# IDE
.vscode/
.idea/
*.swp
.DS_Store
```

## Completed Step

You now have Terraform correctly installed and configured, with your project ready to initialise its first infrastructure build.