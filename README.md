# Automating Azure Infrastructure with Terraform and Azure DevOps

This project focuses on solving the challenges of manual infrastructure provisioning by introducing Infrastructure as Code using Terraform. Traditionally, setting up cloud 
environments is time-consuming, error-prone, and difficult to scale or replicate across multiple environments. By using Terraform, we define the desired infrastructure in 
configuration files, enabling automated, consistent, and repeatable deployment. The goal is to streamline the provisioning process, reduce human error, and make infrastructure 
changes version-controlled and manageable through code.

![image](https://github.com/user-attachments/assets/2fa2af82-30b4-4528-9a47-c737a083f9ca)


# 🚀 Infrastructure Automation with Terraform and Azure DevOps

This project showcases a practical implementation of Infrastructure as Code (IaC) using **Terraform** and **Azure DevOps**. The goal is to automate the provisioning and management of Azure infrastructure without any manual intervention. By integrating Terraform with a DevOps pipeline, infrastructure deployments become faster, more consistent, and fully reproducible.

Instead of manually running Terraform commands every time infrastructure changes are needed, this solution wraps the Terraform lifecycle (`init → plan → apply`) inside an Azure DevOps pipeline. Any change in Terraform configuration triggers the pipeline, which automatically updates the cloud environment based on the declared state.

---

## 🔧 Key Objectives

- Automate Azure infrastructure provisioning using Terraform.
- Implement a CI/CD pipeline with Azure DevOps to handle Terraform operations.
- Store the Terraform state file in a secure Azure backend (Storage Account).
- Maintain version-controlled infrastructure definitions.
- Enable scalable and repeatable infrastructure deployments with minimal manual work.

---

## 🔁 Workflow Overview

1. **Write Terraform Configuration**
   - Define your Azure resources using `.tf` files (e.g., `main.tf`, `variables.tf`).

2. **Configure Backend**
   - Store the Terraform state file in an Azure Storage Account for state management and locking.

3. **Azure DevOps Pipeline Execution**
   - Detects changes in the Terraform code.
   - Automatically runs the Terraform lifecycle:
     - `terraform init`: Initializes providers and backend.
     - `terraform plan`: Detects changes between config and current state.
     - `terraform apply`: Provisions or updates the infrastructure.
   - Updates the backend state file after successful deployment.

4. **Optional Manual Approvals**
   - Approval gates or pull request workflows can be included for safe infrastructure change.
  


The following diagram illustrates how we are automating the Terraform process using Azure Pipelines.

![image](https://github.com/user-attachments/assets/e1065708-5a35-43b1-9022-804bdcb25c94)



First, we need to create principal to perform the terraform actions. Follow below link for that.

```shell
https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/guides/service_principal_client_secret
```

After creating service principal, pull the below repository using git.
```shell
git@github.com:Ajay-Kumar2103/Azure_Terraform.git
```

Initially, run Terraform locally to ensure it works as expected. Once verified, import the configuration into your Azure DevOps project to create a pipeline for automating the Terraform workflow. Before proceeding, ensure that the Terraform state file is stored remotely and securely to prevent accidental deletion along with other resources.





