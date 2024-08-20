### Question 1: What is the purpose of the terraform plan command?
<details>
- The terraform plan command is used to create an execution plan. It shows what actions Terraform will take to achieve the desired state defined in the configuration files. This includes:

Creating: Resources that will be created.

Updating: Resources that will be modified.

Destroying: Resources that will be removed.

The plan output helps users understand the changes before applying them, reducing the risk of unintended modifications.

</details>

### Question 2: what tf state ?
<details>
- TF state (Terraform state) is a file that keeps track of the infrastructure
- State file is crucial because it allows Terraform to know the current status of the infrastructure and how it corresponds to the configuration described in the Terraform files.
</details>

### Question 3: if someone has locked tf state and u want to unlock it on azure patform?
<details>


### 1. **Remove Lock from Azure**
   - If the Terraform lock persists due to an Azure environment or storage issue, you can remove the lock manually from the Azure Storage Account by deleting the lock file associated with your Terraform state:
     - Navigate to the container in the Azure Storage Account where the `.tfstate` file is stored.
     - Locate and delete the lock file, usually named something like `default.tfstate.lock`.


Terraform state locks occur to prevent concurrent operations on the same state file, which could potentially lead to state corruption. Here are some common reasons why a Terraform state might become locked:

### 1. **Concurrent Terraform Operations**
   - **Multiple Users or CI/CD Pipelines**: If multiple users or CI/CD pipelines attempt to run Terraform operations (`apply`, `plan`, `destroy`) simultaneously, Terraform will lock the state to ensure only one process modifies it at a time.

### 2. **Long-Running Terraform Operations**
   - **Complex Changes**: Some Terraform operations might take a long time due to the complexity or number of resources being modified, leading to the state being locked for an extended period.
   - **Network Latency or Timeouts**: High network latency or timeouts can cause Terraform to lose connection with the state, potentially leaving it in a locked state.

</details>
