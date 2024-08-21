

### Question 1: You need to manage multiple environments (e.g., dev, staging, production) using Terraform. How would you structure your Terraform configuration to ensure consistency across environments while allowing environment-specific customizations?
<details>

- Modules: Create reusable modules for components like networks, compute instances, and storage that are shared across environments. These modules are stored in the modules/ directory.
- Use terraform.tfvars within each environment directory to define environment-specific variables (e.g., instance sizes, number of instances, or environment-specific tags).
- Define the backend.tf file in each environment's directory to specify the remote backend for storing the Terraform state files.
- Store your Terraform configurations in a version control system like Git. Each environment can be managed in a separate branch if necessary.
</details>

### Question 2: You accidentally committed your Terraform state file to a public repository. What are the risks involved, and how would you mitigate this issue?

<details>

- Immediately Remove the State File from the Repository
- Revoke Compromised Credentials: Immediately revoke any exposed API keys, provider credentials, or secrets found in the state file.
- Rotate Credentials: Generate new credentials and update your Terraform provider configurations to use them.
- Review Logs: Check cloud provider logs for any unauthorized access or suspicious activity that may have occurred using the exposed credentials
</details>

### Question 3. You are using a remote backend with S3 for storing the Terraform state file and DynamoDB for state locking. Suddenly, a `terraform apply' command is blocked because the state is locked. How would you resolve this?
<details>

- Only manually remove the lock if you are certain that no other Terraform operations are running, as this can lead to state corruption if done incorrectly.

- Remove the Lock Using AWS Management Console:
Select the locked item in the DynamoDB table and delete it.

</details>
