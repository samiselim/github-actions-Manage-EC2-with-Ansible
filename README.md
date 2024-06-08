# Ansible Deployment Workflow

This GitHub Actions workflow automates the deployment process using Ansible. It includes steps for caching dependencies, cloning the repository, installing Ansible and required modules, configuring AWS credentials, and running Ansible playbooks.

## Workflow Overview

### Ansible Deployment Workflow

#### Workflow Trigger
The workflow is manually triggered using `workflow_dispatch`.

#### Environment Variables
- `ANSIBLE_HOST_KEY_CHECKING`: Set to `false` to disable host key checking.
- `ANSIBLE_REMOTE_USER`: Set to `ec2-user` as the remote user for Ansible.

#### Permissions
- `id-token`: Set to `write`.

#### Job: ansible-deploy

##### Runs on
- `ubuntu-latest`

##### Steps

1. **Cache dependencies**
    - Uses GitHub Actions cache to cache Ansible and Python dependencies to speed up workflow runs.

2. **Clone the repository**
    - Clones the repository to the runner.

3. **Install Ansible (latest) and boto3 modules**
    - Installs Ansible and boto3 if they are not already cached.

4. **Install Ansible Galaxy Collection (amazon.aws)**
    - Installs the `amazon.aws` collection from Ansible Galaxy if it is not already cached.

5. **Configure AWS Credentials**
    - Configures AWS credentials from the AWS account to allow Ansible to interact with AWS services.

6. **Run Ansible inventory file**
    - Runs the Ansible inventory file to list the inventory.

7. **Keep SSH key in a file**
    - Saves the SSH key to a file and sets appropriate permissions.

8. **Run Playbook**
    - Runs the Ansible playbook to deploy the required configurations.

## Terraform Action Workflow

This GitHub Actions workflow is designed to build infrastructure using Terraform before running the Ansible deployment workflow. It supports both `apply` and `destroy` actions based on the input provided during workflow dispatch.

### Workflow Trigger
The workflow is manually triggered using `workflow_dispatch` with an input to choose between `apply` and `destroy` actions.

### Environment Variables
- `TF_LOG`: Set to `INFO` to enable logging.
- `AWS_REGION`: AWS region, set using secrets.

### Permissions
- `id-token`: Set to `write`.
- `contents`: Set to `read`.

### Jobs

#### Job: apply

##### Runs on
- `ubuntu-latest`

##### Steps

1. **Git Checkout**
    - Clones the repository to the runner.

2. **Configure AWS Credentials**
    - Configures AWS credentials from the AWS account.

3. **Setup Terraform**
    - Sets up Terraform using the HashiCorp setup action.

4. **Terraform init**
    - Initializes Terraform with backend configuration.

5. **Terraform Validate**
    - Validates the Terraform configuration.

6. **Terraform Plan**
    - Creates a Terraform plan.

7. **Terraform Plan Status**
    - Checks the outcome of the Terraform plan and exits if it fails.

8. **Terraform Apply**
    - Applies the Terraform plan to build the infrastructure.

#### Job: destroy

##### Runs on
- `ubuntu-latest`

##### Steps

1. **Git Checkout**
    - Clones the repository to the runner.

2. **Configure AWS Credentials**
    - Configures AWS credentials from the AWS account.

3. **Setup Terraform**
    - Sets up Terraform using the HashiCorp setup action.

4. **Terraform init**
    - Initializes Terraform with backend configuration.

5. **Terraform Destroy**
    - Destroys the infrastructure managed by Terraform.

## Secrets

The following secrets are required for the workflow:
- `AWS_ROLE`: The AWS role to assume.
- `AWS_REGION`: The AWS region to use.
- `AWS_BACKEND_NAME`: The name of the S3 bucket for the Terraform backend.
- `AWS_STATEFILE_NAME`: The name of the state file in the S3 bucket.
- `ANSIBLE_SSH_KEY`: The SSH key for Ansible.

## Usage

To manually trigger the workflow, go to the "Actions" tab in your GitHub repository, select the "Terraform Action" or "Ansible Deployment" workflow, and click on the "Run workflow" button.
