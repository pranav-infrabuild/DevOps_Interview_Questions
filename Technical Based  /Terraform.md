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
