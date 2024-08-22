
# AWS Infrastructure Management Playbook

## Overview

This project contains an Ansible playbook designed to manage and automate AWS infrastructure tasks such as creating Virtual Private Clouds (VPCs), subnets, Internet Gateways (IGWs), security groups, and EC2 instances. The playbook is structured to be flexible and customizable, allowing it to be easily adapted to different environments by using various collections and survey-based variable inputs.

## Project Structure

- **playbooks/**: Contains the main playbook (`main.yml`) that defines the tasks to be executed.
- **roles/**: Directory for roles used in the playbook. These roles can be part of any Ansible collection, allowing easy customization.
- **vars/**: Contains variable files (e.g., `mapping.yml`) that store configuration values used by the playbooks.
- **inventory/**: Defines the inventory files that list the hosts and groups for the playbooks.
- **surveys/**: Directory for survey definitions that will be used in the Automation Controller to dynamically input variables.

## Playbook Overview

### Purpose

The playbook is designed to handle the lifecycle of AWS infrastructure components, including:

- Creating and managing VPCs and subnets.
- Setting up Internet Gateways and security groups.
- Launching EC2 instances and attaching EBS volumes.
- Optionally tearing down the created resources.

### Key Features

- **Modular Design**: The playbook is modular and can be easily extended or modified to include additional tasks or roles.
- **Dynamic Variables**: All key variables are designed to be provided via surveys in the Automation Controller, making the playbook highly flexible.
- **Customizable Roles**: The playbook can use roles from different Ansible collections. The role names and collections can be swapped by simply updating the playbook and the survey variables.

## Usage

### Running the Playbook

To run the playbook, you need to set up the necessary variables, which are typically provided through a survey in the Automation Controller. The playbook can be executed manually as follows:

```bash
ansible-playbook aws_playbook.yml
```

### Customizing the Playbook

#### Collection and Role Customization

The playbook is designed to use roles from an Ansible collection, which can be easily swapped out. The role names are defined generically(check the collections dir to find the collection and role name to run your playbook), and you can replace them with the appropriate role from your collection. For example:

```yaml
- hosts: localhost
  roles:
    - { role: "collection_namespace.role_name" }
```

Replace `collection_namespace.role_name` with the actual collection and role name you intend to use.(check the collections dir to find the collection and role name to run your playbook)

#### Variable Management via Surveys

All variables required by the playbook, such as `instance_type`, `region`, `key_name`, etc., are defined in survey files located in the `surveys/` directory. These surveys are intended to be used in the Automation Controller to dynamically provide input to the playbook.

Example Survey Variables:

- **`instance_type`**: The type of EC2 instance to launch.
- **`region`**: AWS region where resources will be created.
- **`key_name`**: Name of the SSH key pair.
- **`volume_size`**: Size of the EBS volume to attach.
- **`ami_os`**: Operating system for the EC2 instance.
- **`survey_teardown_response`**: Determines if resources should be torn down after creation.

### Teardown Process

The playbook includes a teardown process, controlled by the `survey_teardown_response` variable. If set to `yes`, the playbook will remove the created AWS resources.

## Integration with Automation Controller

### Surveys

The project includes predefined surveys that can be imported into the Automation Controller. These surveys will prompt users to input values for variables like `instance_type`, `region`, and more. The playbook is designed to work seamlessly with these survey inputs.

### Example Survey Setup
This project automatically sets up a Job Template with a survey attached in the Blueprint Automation Team's automation controller, to edit the survey, job templates or project, Please edit the controller_configuration dir
1. **Create a Job Template** in Automation Controller.
2. **Attach the Survey** to the job template, ensuring that the survey variables match those used in the playbook.
3. **Run the Playbook** using the job template, providing dynamic inputs through the survey.

## Best Practices

- **Keep It Modular**: Organize your roles and tasks in a modular way to ensure easy maintenance and customization.
- **Use Surveys**: Leverage the power of surveys to keep your playbooks flexible and adaptable to different environments.
- **Idempotency**: Ensure that your playbook is idempotent, meaning it can be run multiple times without causing unintended changes.

## Troubleshooting

- **AWS Credentials**: Make sure AWS credentials are correctly configured on the control node.
- **Survey Inputs**: Double-check that all required survey inputs are provided correctly when running the playbook.
- **Playbook Syntax**: Use `ansible-playbook --syntax-check` to validate the syntax before execution.

## Contributing

Contributions to this project are welcome. Please see the `CONTRIBUTING.md` file for guidelines on how to contribute.

## License

This project is licensed under the MIT License. See the `LICENSE` file for more details.

## Contact

For any questions or support, please contact the project maintainers.
