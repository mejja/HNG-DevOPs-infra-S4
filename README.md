# Terraform + Ansible EC2 Deployment

This project automates the deployment of a containerized application on AWS by:
- Provisioning an EC2 instance with Terraform.
- Installing Docker, Git, Docker Compose, and required tools on the instance with Ansible.
- Cloning a repository and starting the application using `docker-compose up -d`.

## Folder Structure

```bash
.
├── README.md
├── .env                     # Environment variables for our services
├── terraform/               # Terraform configuration files
│   ├── main.tf              # Main Terraform configuration
│   ├── provider.tf          # AWS provider configuration
│   ├── variables.tf         # Variable definitions
│   └── outputs.tf           # Outputs (e.g., instance public IP)
└── ansible/                 # Ansible playbook directory
    ├── playbook.yml         # Ansible playbook to provision the instance
    └── roles/
        ├── dependencies/
        │   └── tasks/ 
        │      └── main.yml
        └── deployment/
            └── tasks/
                └── main.yml 

```

## Prerequisites

- **Terraform** (v0.12 or later)
- **Ansible** (v2.9 or later)
- **AWS CLI** (configured with proper credentials)
- An AWS account (with permissions to create EC2 instances, security groups, etc.)
- A public SSH key (e.g., `~/.ssh/id_rsa.pub`) for connecting to the EC2 instance

## Environment Variables

Create a `.env` file in the root of the repository with the following contents (adjust as needed):

```ini
# AWS Settings
AWS_REGION=us-east-1
```

# Application Service Settings

## Deployment Process

1. **Terraform Provisioning:**
   - Terraform in the `terraform/` directory provisions an EC2 instance and a security group.
   - Outputs the instance’s public IP.
   - A null_resource triggers an Ansible playbook once the instance is up.

2. **Ansible Provisioning:**
   - The Ansible playbook in `ansible/playbook.yml` connects to the EC2 instance.
   - It installs Docker, Git, Docker Compose, and other dependencies.
   - It adds the default user (e.g., `ubuntu`) to the docker group.
   - It clones your repository.
   - It runs `docker-compose up -d` in the repository directory to start your application.

## Deployment Instructions

1. **Clone this repository:**

   ```bash
   git clone https://github.com/yourusername/yourrepo.git
   cd yourrepo
   ```

2. **Ensure the `.env` file is configured correctly.**

3. **Run the deployment with Terraform:**

   ```bash
   terraform apply -auto-approve
   ```

   This will provision the infrastructure and run the Ansible playbook on the new EC2 instance.

4. **Verify the Deployment:**
   - Check the Terraform output for the EC2 instance's public IP.
   - SSH into the instance (if needed) to verify that Docker containers are running.
   - Access your application endpoints as defined in your Docker Compose and Traefik configuration.

## Cleanup

To tear down the infrastructure, run:

```bash
terraform destroy -auto-approve
```

## Troubleshooting

- **Terraform Errors:**  
  Ensure your AWS credentials have the necessary permissions.
- **Ansible Errors:**  
  Check the output from the Ansible playbook for installation or configuration issues.
- **Access Issues:**  
  Verify that AWS security groups allow SSH and the required application ports.
- **DNS/Routing Issues:**  
  Confirm that your DNS records and Traefik configuration correctly route requests to your services.

---

Happy deploying!
```

