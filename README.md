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



1. **Write & Configure Terraform Configuration**
   
   The terraform files has been updated in repository and use them to configure the infrastructure. After running "terraform apply" command the resources will be created in azure portal. Once everything is created then we can assume that it is working and then use "Terraform destory" to delete all the resources. 



2. **Azure DevOps Pipeline Execution**

   - Import the repository to Azure DevOps Organisation
   - Create a new basic pipeline and use "azure pipeline.yaml" for building pipeline.
   - Create a storage account, then container to save the artifacts and to use for "release pipeline".

   ![image](https://github.com/user-attachments/assets/55213d3b-6f45-43a3-bb59-c94e082ca263)


   - Save the pipeline and run it. It includes terraform install, init, validate, format, plan, archive and publish. This will help us to extract the published artifact later and in release pipeline to destroy the infrastructure.

![image](https://github.com/user-attachments/assets/050c8d6d-472b-4e2f-9bb4-f6baa1c1bdc0)



![image](https://github.com/user-attachments/assets/d87d6d10-30f7-4d47-945f-7d0d1e440cdc)



The above image clearly shows that the pipeline ran successfully and we can proceed for next step which is "release pipeline"


Create the release pipeline in below format.

Select Release option, then create a new deployment.

![image](https://github.com/user-attachments/assets/fb4fab22-8b27-4d39-8181-4fed00f7f266)

![image](https://github.com/user-attachments/assets/e7868495-dd29-49ee-9eb2-7316e3d1b15e)


![image](https://github.com/user-attachments/assets/6bf04b1b-e97b-4935-bf58-da44f1806aa0)


![image](https://github.com/user-attachments/assets/3dbecdc5-8ffa-4d0c-86f7-a16796c48654)


Once everything has been set, click on deploy option to deploy the it.



Once Artifact has been installed and successfully extracted, it looks as follows

![image](https://github.com/user-attachments/assets/00c5852a-a4ee-4293-b9d1-b4ef14043ae6)



Now we can create "Destroy" stage. You can copy the deployment stage and need to make a minor change in it. Instead of terraform apply replace it with "terraform destroy"

![image](https://github.com/user-attachments/assets/ed32585a-03db-4607-9a99-3e284bb54652)

![image](https://github.com/user-attachments/assets/12772d58-b4de-4475-b061-ab0ccd5fa8ad)


We need to make sure that we provide "--auto-approve" in additional comments which will eliminate the need of manual intervetion.


Once everything is set up, we can make a change to trigger pipeline which will make to provision the enviorment and the release pipeline will be triggered. We have also made an approval section for destroying the environment so that it won't destroy accidentally.

![image](https://github.com/user-attachments/assets/cd92c018-8245-406d-a526-0c915bfcd318)


As long as we have the teraform files, we can recreate the environent as many times as possible, but the data inside them can't be restored. So we need to proceed with caution while destroying environment.


















