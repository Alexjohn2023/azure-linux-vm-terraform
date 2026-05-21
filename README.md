# Azure Linux VM Deployment using Terraform

![Terraform](https://img.shields.io/badge/Terraform-IaC-blueviolet)
![Azure](https://img.shields.io/badge/Azure-Cloud-blue)
![Linux](https://img.shields.io/badge/Linux-Ubuntu-green)
![SSH](https://img.shields.io/badge/SSH-Enabled-success)

Production-pattern Azure infrastructure provisioned entirely through 
Terraform IaC — no manual portal clicks. Includes remote state 
management, NSG-secured networking, and passwordless SSH access.

---

## Why This Project

Most tutorials stop at `terraform apply`. This project goes further:

- Remote state stored in Azure Blob Storage — infrastructure is 
  team-ready and drift-resistant
- Troubleshot real deployment failures: NSG misassociation, SKU 
  availability conflicts, SSH connectivity issues
- SSH alias configuration (`ssh web1`) for repeatable, 
  production-style access

---

## Architecture
Internet → Public IP → NSG (Allow SSH:22) → NIC → Linux VM (Ubuntu)
↑
Terraform Remote State
(Azure Blob Storage)

**Network:**
- VNet: `10.0.0.0/16`
- Subnet: `10.0.1.0/24`

**Resources deployed:**
| Resource | Name |
|---|---|
| Resource Group | web1-rg |
| Virtual Network | web1-vnet |
| Subnet | web1-subnet |
| Public IP | web1-public-ip |
| NSG | web1-nsg |
| Network Interface | web1-nic |
| Linux VM | web1 |
| State Backend | tfstatebackend026 / tfstate container |

---

## Prerequisites

- [Terraform](https://developer.hashicorp.com/terraform/install) v1.x+
- [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) 
  (authenticated via `az login`)
- SSH key pair generated (`ssh-keygen -t rsa -b 4096`)
- Azure subscription with Contributor access

---

## Remote State Setup (do this first)

Before running `terraform init`, create the backend storage manually 
or via Azure CLI:

```bash
# Create resource group for state
az group create --name tfstate-rg --location eastus

# Create storage account
az storage account create \
  --name tfstatebackend026 \
  --resource-group tfstate-rg \
  --sku Standard_LRS

# Create container
az storage container create \
  --name tfstate \
  --account-name tfstatebackend026
```

---

## Deployment

```bash
# 1. Initialize with remote backend
terraform init

# 2. Review planned changes
terraform plan

# 3. Deploy
terraform apply

# 4. Connect (after deployment)
ssh web1

# 5. Tear down
terraform destroy
```

---

## SSH Alias Configuration

Add to `~/.ssh/config`:
Host web1
HostName <your-public-ip>
User azureuser
IdentityFile ~/.ssh/id_rsa

---

## Troubleshooting Notes

Real issues encountered and resolved during this deployment:

**NSG not blocking SSH correctly**
- Root cause: NSG was created but not associated to the NIC
- Fix: Explicit `network_interface_security_group_association` 
  resource in `main.tf`

**SKU availability error on VM size**
- Root cause: Selected VM SKU not available in target region/zone
- Fix: Queried available SKUs with 
  `az vm list-skus --location eastus --output table` 
  and updated `variables.tf`

**SSH connection refused after deployment**
- Root cause: Public IP assigned but NSG rule priority conflict
- Fix: Verified rule priority ordering and re-applied

---

## Repository Structure
azure-linux-vm-terraform/
├── main.tf           # All resource definitions
├── provider.tf       # Azure provider + remote backend config
├── providers.tf      # Provider version constraints
├── variables.tf      # Input variables
├── outputs.tf        # Public IP and VM outputs
├── .gitignore
├── screenshots/      # Deployment evidence
└── README.md

---

## Screenshots

| VS Code + Live SSH Session | Azure Resource Group | Remote State (Blob) |
|---|---|---|
| ![ssh](screenshots/vscode-ssh.png) | ![rg](screenshots/resource-group.png) | ![state](screenshots/tfstate-blob.png) |

---

## Status

✅ Completed — May 2026

---

## Author

Alexander Njoku — Cloud & DevOps Engineer  
[LinkedIn](https://linkedin.com/in/alexander-njoku-62040a194) · 
[Medium](https://medium.com/@alex2020global) · 
[Portfolio](https://zandersworldview.com)
