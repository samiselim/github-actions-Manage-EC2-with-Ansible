# Ansible Deployment Workflow

This GitHub Actions workflow automates the deployment process using Ansible. It includes steps for caching dependencies, cloning the repository, installing Ansible and required modules, configuring AWS credentials, and running Ansible playbooks.

## Workflow Overview

### Workflow Trigger
The workflow is manually triggered using `workflow_dispatch`.

### Environment Variables
- `ANSIBLE_HOST_KEY_CHECKING`: Set to `false` to disable host key checking.
- `ANSIBLE_REMOTE_USER`: Set to `ec2-user` as the remote user for Ansible.

### Permissions
- `id-token`: Set to `write`.

### Job: ansible-deploy

#### Runs on
- `ubuntu-latest`

#### Steps

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

## Secrets

The following secrets are required for the workflow:
- `AWS_ROLE`: The AWS role to assume.
- `AWS_REGION`: The AWS region to use.
- `ANSIBLE_SSH_KEY`: The SSH key for Ansible.

## Usage

To manually trigger the workflow, go to the "Actions" tab in your GitHub repository, select the "Ansible Deployment" workflow, and click on the "Run workflow" button.
