# terraform-aws-microservice-blueprint

> **A one-command, production-ready IaC kit that spins up secure, auto-scaling micro-services on AWS—complete with VPC networking, encrypted storage, and an opinionated CI/CD pipeline.**

&nbsp;

<p align="center">
  <img src="https://img.shields.io/badge/IaC-Terraform%20v1.8%2B-purple" alt="Terraform 1.8+">
  <img src="https://img.shields.io/badge/Cloud-AWS-orange" alt="AWS">
  <img src="https://img.shields.io/badge/License-Apache--2.0-blue.svg" alt="Apache 2.0">
</p>

---

## ✨ Why use this blueprint?

* **From empty account to live services in minutes** – a single `terraform apply` provisions every layer, including CI/CD.  
* **Modular & plug-and-play** – add a new micro-service with two lines in `terraform.tfvars`; no code edits required.  
* **Security & compliance by default** – KMS-encrypted state, S3, ECR; least-privilege IAM; private subnets behind NAT.  
* **Cost-smart defaults** – spot-ready launch templates, right-sized `t3.micro` instances, granular NAT usage.  
* **Zero-downtime blue/green deployments** – CodePipeline + CodeBuild automatically build, scan, push, and hot-swap ALB targets with health-check rollbacks.

---

## 🗺️ What’s inside?

| **Layer**            | **What it delivers**                                                                                                                                                      | **Why it matters**                                                                           |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| **Networking**       | Three-AZ VPC, public & private subnets, per-AZ NAT gateways, ALB security group                                                                                           | High availability out of the box; private workloads never touch the public Internet          |
| **IAM**              | Least-privilege roles for EC2, CodeBuild, CodePipeline, plus optional project KMS key                                                                                     | Every component gets only the access it needs; all artifacts are KMS-encrypted               |
| **Storage**          | S3 artifact bucket (versioned + SSE-KMS) and per-service ECR repositories                                                                                                  | Immutable Docker images and audit-friendly build artifacts                                   |
| **Service**          | ALB, Target Group, Launch Template, and Auto Scaling Group wired for blue/green deploys                                                                                   | Zero-downtime releases, automatic health-check rollbacks, spot-instance support              |
| **CI/CD *(optional)* | CodeCommit → CodeBuild → CodePipeline ↔ ALB                                                                                                                               | One-click pipeline that builds, scans, tags, and rolls forward—or back—safely                |

---

## 🚀 Quick start

```bash
# 1) Clone the repo
git clone https://github.com/<you>/terraform-aws-microservice-blueprint.git
cd terraform-aws-microservice-blueprint

# 2) Edit terraform.tfvars
cat <<EOF > terraform.tfvars
project_name = "demo"
services = [
  { name = "frontend", app_port = 8080, repo_name = "frontend-repo", branch = "main" },
  { name = "newsfeed", app_port = 8081, repo_name = "newsfeed-repo", branch = "main" }
]
EOF

# 3) Provision
terraform init
terraform apply -auto-approve
