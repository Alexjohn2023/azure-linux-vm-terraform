# Azure Linux VM Deployment using Terraform

![Terraform](https://img.shields.io/badge/Terraform-IaC-blueviolet)
![Azure](https://img.shields.io/badge/Azure-Cloud-blue)
![Linux](https://img.shields.io/badge/Linux-Ubuntu-green)
![SSH](https://img.shields.io/badge/SSH-Enabled-success)

Deploy Azure infrastructure using Terraform with Infrastructure as Code (IaC) principles.

## Project Overview

This project provisions an Azure Linux Virtual Machine and supporting network infrastructure using Terraform.

The deployment includes:

- Azure Resource Group
- Virtual Network (VNet)
- Subnet
- Public IP Address
- Network Security Group (NSG)
- Network Interface (NIC)
- Azure Linux Virtual Machine
- SSH Key Authentication
- SSH Alias (`ssh web1`)
- Terraform State Management

---

## Architecture

```text
Internet
   ↓
Public IP
   ↓
NSG
   ↓
NIC
   ↓
Linux VM
```

Network Configuration:

```text
VNet:   10.0.0.0/16
Subnet: 10.0.1.0/24
```

---

## Tech Stack

Terraform | Azure | Linux | SSH | Networking | Infrastructure as Code

---

## Deployment

Initialize Terraform:

```bash
terraform init
```

Review infrastructure plan:

```bash
terraform plan
```

Deploy infrastructure:

```bash
terraform apply
```

SSH access:

```bash
ssh web1
```

Destroy resources:

```bash
terraform destroy
```

---

## Learning Outcomes

Through this project I gained hands-on experience with:

- Azure networking
- Infrastructure as Code (Terraform)
- Linux VM deployment
- SSH authentication
- NSG configuration and troubleshooting
- Cloud infrastructure validation
- End-to-end infrastructure deployment workflow

---

## Repository Structure

```text
azure-linux-vm-terraform/
│
├── main.tf
├── provider.tf
├── providers.tf
├── variables.tf
├── outputs.tf
├── README.md
├── .gitignore
└── screenshots/
```

---

## Project Status

Completed ✅
