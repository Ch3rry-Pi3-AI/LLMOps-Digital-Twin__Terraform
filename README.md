Here is a clean, polished, **professional-style** branch README that matches the tone, structure, and polish of your previous branch readmes — no horizontal lines, no clutter, and fully consistent with your established documentation style.

---

# 🚀 Deploy Development Environment

This branch introduces automated deployment for the **development environment** of the Digital Twin project. The goal is to enable a complete, reproducible deployment flow using Terraform and platform-specific scripts, ensuring the entire stack can be deployed with a single command.

## Overview

In this stage, you:

* Initialise Terraform for the project
* Use deployment scripts to package the backend, provision AWS infrastructure, build the frontend, and upload assets
* Verify that your development environment is fully accessible through CloudFront and API Gateway

These steps replace manual deployment processes and form the foundation for future CI/CD automation.

## Steps Completed in This Branch

### Step 1: Initialise Terraform

From the project root, open a terminal and run:

```bash
cd terraform
terraform init
```

Expected output includes:

```
Initializing the backend...
Initializing provider plugins...
- Finding hashicorp/aws versions matching "~> 6.0"...
- Installing hashicorp/aws v6.23.0...
- Installed hashicorp/aws v6.23.0 (signed by HashiCorp)
Terraform has created a lock file .terraform.lock.hcl ...
Terraform has been successfully initialized!
```

Terraform is now fully initialised and ready to manage the AWS resources required for your Digital Twin.

### Step 2: Deploy Using the Script

The deployment scripts automate the entire workflow: Lambda packaging, Terraform provisioning, frontend build, and S3 synchronisation.

macOS / Linux users:

```bash
./scripts/deploy.sh dev
```

Windows PowerShell users:

```powershell
.\scripts\deploy.ps1 -Environment dev
```

During execution, the script will:

1. Build the backend Lambda package
2. Create or select the Terraform workspace (`dev`)
3. Deploy the complete AWS infrastructure
4. Build the frontend static export
5. Upload the frontend to the S3 website bucket
6. Output all relevant CloudFront and API URLs

### Step 3: Test the Deployed Environment

Once deployment completes successfully:

1. Open the **CloudFront URL** printed in the terminal
2. Confirm that the frontend loads
3. Send a message through the chat interface to verify backend integration

Your Digital Twin should now be fully operational in the **development environment**, deployed entirely via the new automated workflow.

## Summary

This branch establishes a unified, automated deployment flow across macOS, Linux, and Windows. With the deployment scripts and Terraform configuration now in place, you can deploy or update the entire environment reliably with a single command.

The project is now ready for the next phase: extending this automation into full CI/CD.
