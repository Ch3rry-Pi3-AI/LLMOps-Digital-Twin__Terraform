# 🏗️ **LLMOps Digital Twin — Infrastructure Automation (Terraform Edition)**

The **LLMOps Digital Twin — Terraform Automation Project** extends the original Digital Twin system by introducing a **fully automated, infrastructure-as-code workflow** for provisioning and managing the entire AWS architecture.

Instead of manually configuring Lambda, API Gateway, S3, and CloudFront, this project enables:

* **One-command deployment** of all AWS resources
* **Workspace-based multi-environment support** (dev, test, prod)
* **Automated S3 bucket creation, IAM roles, CloudFront distribution, and API Gateway**
* **Consistent, repeatable, error-free deployments**
* **Automated frontend + backend deployment pipeline**
* **Full environment teardown via destroy scripts**

The result is a robust, professional-grade Terraform-based workflow suitable for real LLMOps production systems.

## 🎥 **Digital Twin Demo**

<div align="center">
  <img src="img/demo/twin_demo.gif" width="100%" alt="Digital Twin Demo">
</div>

The Terraform project manages all AWS infrastructure required to run this Digital Twin end-to-end.

## 🧩 **Grouped Stages**

This Terraform extension consists of **six stages**, grouped to reflect the natural lifecycle of IaC-driven AWS deployment:

| Stage Group | Category                | Description                                                                                        |
| :---------: | ----------------------- | -------------------------------------------------------------------------------------------------- |
|    **00**   | Clean Slate             | Removal of all manually created AWS resources to ensure Terraform has full ownership               |
|    **01**   | Terraform Installation  | Installing Terraform on macOS, Linux, or Windows; configuring PATH; verifying installation         |
|    **02**   | Terraform Configuration | Creating versions, variables, main infrastructure files, and output definitions                    |
|    **03**   | Deployment Scripts      | Shell + PowerShell automation for packaging Lambda, applying Terraform, and deploying the frontend |
|    **04**   | Deploy Dev Environment  | Terraform init, workspace creation, full end-to-end deployment of the development environment      |
|    **05**   | Destroy Infrastructure  | Automatic teardown scripts for safely removing all resources and emptying S3 buckets               |

This workflow provides the complete lifecycle:
**clean state → automated provisioning → deployment → teardown**.

## 🗂️ **Project Structure**

```
LLMOps-Digital-Twin__Terraform/
├── backend/
│   ├── server.py
│   ├── lambda_handler.py
│   ├── context.py
│   ├── resources.py
│   ├── deploy.py
│   ├── data/
│   ├── requirements.txt
│   └── README.md
├── frontend/
│   ├── app/
│   ├── components/
│   ├── public/
│   ├── out/
│   ├── package.json
│   └── README.md
├── terraform/
│   ├── versions.tf
│   ├── variables.tf
│   ├── main.tf
│   ├── outputs.tf
│   ├── terraform.tfvars
│   └── prod.tfvars (optional)
├── scripts/
│   ├── deploy.sh
│   ├── deploy.ps1
│   ├── destroy.sh
│   └── destroy.ps1
├── img/
│   └── demo/
│       └── twin_demo.gif
├── .gitignore
└── README.md
```

## 🧠 **Core Components of the Terraform System**

### 🧱 Terraform Configuration (versions.tf, variables.tf, main.tf)

Terraform provisions:

* AWS Lambda (Python 3.12)

* API Gateway (HTTP API)

* CloudFront distribution

* S3 frontend bucket (static site hosting)

* S3 memory bucket (Digital Twin conversation memory)

* IAM roles + permissions for:

  * Lambda execution
  * S3 access
  * Bedrock access

* Optional custom domain (ACM + Route 53)

All resources are tagged, environment-scoped, and fully modular.

### 🧩 Variable System (`variables.tf` & `terraform.tfvars`)

The variable files enable consistent parameterisation:

* project name
* environment (dev/test/prod)
* Bedrock model ID
* Lambda timeout
* API throttle limits
* Optional custom domain

Workspaces keep environments isolated and safe.

### 🧪 Deployment Scripts (`deploy.sh`, `deploy.ps1`)

The scripts:

1. Build the Lambda package
2. Initialise or select Terraform workspace
3. Apply Terraform with correct vars
4. Retrieve CloudFront + API Gateway URLs
5. Build and upload the frontend to S3
6. Produce a clean summary output

These scripts provide **true one-command deployment**.

### 🔥 Destroy Scripts (`destroy.sh`, `destroy.ps1`)

The teardown scripts:

* Automatically empty S3 buckets
* Destroy resources with Terraform
* Keep workspaces (optional to delete)
* Provide clean output of what was removed

This guarantees cost control and prevents AWS resource drift.

## 💻 **Local Development**

Backend:

```bash
cd backend
pip install -r requirements.txt
uvicorn server:app --reload
```

Frontend:

```bash
cd frontend
npm install
npm run dev
```

Lambda packaging (used by deploy scripts):

```bash
cd backend
uv run deploy.py
```

## 🚀 **Deploying Environments (dev/test/prod)**

Mac/Linux:

```bash
./scripts/deploy.sh dev
```

Windows PowerShell:

```powershell
.\scripts\deploy.ps1 -Environment dev
```

The script will:

1. Package Lambda
2. Apply Terraform
3. Build and upload frontend
4. Print URLs:

   * CloudFront URL
   * API Gateway URL
   * Custom domain (if enabled)

## 🗑️ **Destroying an Environment**

Mac/Linux:

```bash
./scripts/destroy.sh dev
```

Windows:

```powershell
.\scripts\destroy.ps1 -Environment dev
```

What gets removed:

* CloudFront
* Lambda
* API Gateway
* S3 buckets
* IAM roles
* Route 53 records
* ACM certificates

Everything created by Terraform is destroyed safely and cleanly.

## **Summary**

This project transforms the previously manual Digital Twin deployment into a **fully automated, Terraform-driven, multi-environment IaC system**, giving you:

* Reliable reproducibility
* One-command deployments
* Safe environment destruction
* Professional AWS IaC architecture
* Full integration with your Bedrock-powered Digital Twin

The Digital Twin is now backed by **end-to-end AWS automation** — ready for production, scaling, CI/CD integration, and long-term maintainability.
