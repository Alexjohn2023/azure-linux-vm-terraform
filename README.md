# Azure Linux VM Deployment using Terraform

Deploy Azure infrastructure using Terraform with Infrastructure as Code principles.

## Features

- Azure Resource Group
- Virtual Network (VNet)
- Subnet
- Public IP
- Network Security Group (NSG)
- Network Interface
- Azure Linux Virtual Machine
- SSH Key Authentication
- SSH Alias (`ssh web1`)
- Terraform State Management

## Architecture

Internet → Public IP → NSG → NIC → Linux VM

VNet: 10.0.0.0/16  
Subnet: 10.0.1.0/24

## Tech Stack

Terraform | Azure | Linux | SSH | IaC | Networking

## Deployment

```bash
terraform init
terraform plan
terraform apply
