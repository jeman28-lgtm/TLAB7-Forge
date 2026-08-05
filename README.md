# 🛡️ TLAB 7: The Automated Forge

## 📌 Overview
This lab demonstrates an automated, keyless DevSecOps continuous deployment pipeline. Using **GitHub Actions** federated with **AWS IAM via OIDC**, infrastructure is scanned for security vulnerabilities using **tfsec (SAST)** before being provisioned with **Terraform**.

---

## 🏗️ Architecture & Workflow

1. **Authentication (OIDC Federation):** Keyless authentication between GitHub Actions and AWS IAM using OpenID Connect (OIDC).
2. **SAST Quality Gate:** `tfsec` scans the Terraform configuration files for security flaws (e.g., overly permissive ingress rules).
3. **Deployment:** Infrastructure is only applied (`terraform apply`) if zero high-severity security violations are found.

[ Push to main ] ➔ [ OIDC Auth ] ➔ [ tfsec Scanner ] ➔ [ Terraform Apply ]
                                           │
                                    (Fails if unsafe)

---

## 🛡️ Security Remediation Highlight
* **Initial Finding:** `aws-vpc-no-public-ingress-sgr` — Security group ingress allowed unrestricted public access (`0.0.0.0/0`).
* **Remediation:** Updated ingress rules in `main.tf` to restrict access strictly to a specific host IP address (`/32`).

---

## 🚀 How to Run Locally

```bash
# Navigate to the lab directory
cd TLAB7

# Initialize Terraform
terraform init

# Run security scan locally
tfsec .

# Apply configuration
terraform apply

# Destroy resources when complete
terraform destroy -auto-approve
