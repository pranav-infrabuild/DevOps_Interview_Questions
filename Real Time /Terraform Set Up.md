## How to Set up Terraform in real time Organisation 

To set up Terraform for Azure DevOps in a real-time organizational setting, and authenticate it with your project while using the **Azure DevOps Classic Editor**, you need to follow several steps. These involve configuring Azure DevOps pipelines, authenticating Terraform with Azure, and ensuring proper access control.

Here’s a step-by-step guide:

### 1. **Set Up Azure DevOps Project and Repository**
First, ensure you have:
- An **Azure DevOps project** created.
- A **Git repository** in Azure DevOps to store your Terraform code.

### 2. **Install Terraform in Azure Pipelines**
In Azure DevOps, you can integrate Terraform using a pipeline that either installs Terraform manually or uses a pre-installed Azure Pipelines agent that has Terraform.

#### Steps:
1. **Go to your Azure DevOps Project** and navigate to the Pipelines section.
2. Select **New Pipeline** -> **Classic Editor**.
3. Choose where your repository is located (typically, "Azure Repos Git").
4. Choose the pipeline template (select "Empty job").

### 3. **Configure Service Connection for Azure Authentication**
Terraform needs to authenticate with Azure to provision resources. You will use a **Service Principal** to establish this connection.

#### Steps:
1. **Create a Service Principal**:
   - Open the **Azure Portal**.
   - Navigate to **Azure Active Directory** > **App registrations** > **New registration**.
   - Name your Service Principal (e.g., `Terraform-SP`) and select the supported account types.
   - After creating, go to **Certificates & Secrets** and generate a **Client Secret**.
   - In the **API permissions**, ensure you grant access to Azure resources (Azure Resource Management permissions).
   - Under **Overview**, note the **Application (Client) ID** and **Directory (Tenant) ID**.

2. **Assign Roles** to the Service Principal:
   - Navigate to your **Azure subscription** > **Access control (IAM)** > **Role assignments**.
   - Assign the Service Principal the **Contributor** role at the subscription level or at the specific resource group level.

3. **Create a Service Connection in Azure DevOps**:
   - In Azure DevOps, go to your **Project Settings** > **Service Connections** > **New Service Connection**.
   - Select **Azure Resource Manager** and authenticate using your **Service Principal** credentials (Client ID, Tenant ID, and Client Secret).
   - Name the connection (e.g., `Terraform-Service-Connection`).

### 4. **Create an Azure DevOps Pipeline Using Classic Editor**
You’ll now configure the pipeline to run Terraform commands. Use the **Terraform tasks** to execute commands like `init`, `plan`, and `apply`.

#### Example Steps:
1. In the **classic pipeline editor**, add a task for each Terraform operation:
   
   - **Install Terraform**:
     - Add a "Command Line" task (if Terraform isn’t pre-installed).
     - Command: `sudo apt-get install -y terraform` (for Linux agents) or equivalent for Windows agents.
   
   - **Initialize Terraform**:
     - Add a "Command Line" task to run `terraform init`.
     - Working Directory: The folder containing your Terraform files.
   
   - **Terraform Plan**:
     - Add another "Command Line" task for `terraform plan`.
     - Add the arguments for your Azure subscription, resource group, and other variables.
   
   - **Terraform Apply**:
     - Add a "Command Line" task for `terraform apply -auto-approve`.

   **Example YAML** (if you prefer YAML pipelines):
   ```yaml
   pool:
     vmImage: 'ubuntu-latest'

   steps:
   - task: UseTerraform@0
     inputs:
       command: 'init'
       workingDirectory: '$(System.DefaultWorkingDirectory)/terraform'
       backendServiceArm: 'Terraform-Service-Connection'

   - task: UseTerraform@0
     inputs:
       command: 'plan'
       workingDirectory: '$(System.DefaultWorkingDirectory)/terraform'
       backendServiceArm: 'Terraform-Service-Connection'
       additionalArgs: '-out=tfplan'

   - task: UseTerraform@0
     inputs:
       command: 'apply'
       workingDirectory: '$(System.DefaultWorkingDirectory)/terraform'
       backendServiceArm: 'Terraform-Service-Connection'
       additionalArgs: 'tfplan'
   ```

   **Note**: In the classic editor, you’ll configure these tasks using UI options.

### 5. **Store and Use Terraform State in Azure Blob Storage**
It’s a best practice to store Terraform state files in **Azure Blob Storage** for better security and collaboration.

#### Steps:
1. In your Terraform code, configure the **backend** to use Azure storage:
   ```hcl
   terraform {
     backend "azurerm" {
       storage_account_name = "yourstorageaccount"
       container_name       = "yourcontainer"
       key                  = "terraform.tfstate"
       resource_group_name  = "yourResourceGroup"
       subscription_id      = "yourSubscriptionID"
     }
   }
   ```

2. Ensure the **Service Principal** used by your pipeline has access to this storage account (Contributor role on the Resource Group or Storage Account level).

### 6. **Authentication with Azure DevOps Project**
For authentication within the project, ensure:
- The **Azure DevOps Service Connection** (created earlier) is selected in the pipeline for Terraform tasks.
- The **Service Principal** has necessary roles like **Contributor** to deploy resources on the target Azure Subscription.

### 7. **Run the Pipeline**
Once your pipeline is configured, save it, and queue the pipeline run. The pipeline will execute the Terraform commands and authenticate using the service connection.

### Key Tips:
- **Environment Variables**: You can pass sensitive information (like client secrets) as environment variables in the pipeline.
- **Variable Groups**: Store commonly used variables (like subscription IDs, client secrets) in a Variable Group in Azure Pipelines for reuse across pipelines.
- **Terraform Cloud**: If needed, you can integrate **Terraform Cloud** for remote state management and better collaboration.

By following these steps, you’ll be able to set up Terraform in Azure DevOps, authenticate it with your project, and automate infrastructure provisioning using pipelines configured with the Classic Editor.
