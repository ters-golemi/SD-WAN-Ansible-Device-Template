# SD-WAN Ansible Device Template Management

This repository contains a comprehensive lab guide and Ansible playbooks for managing Cisco Catalyst SD-WAN devices using Ansible with Cisco SD-WAN Manager 20.15.

## Overview

Learn how to automate SD-WAN device template management using Ansible. This lab covers:

- Checking for compatible device templates for vSmart controllers
- Listing vSmart controllers in your SD-WAN fabric
- Attaching vSmart controllers to device templates with specific variables
- Verifying template attachment and configuration synchronization

## Quick Start

1. Review the complete lab guide: [LAB_GUIDE.md](LAB_GUIDE.md)

2. Update inventory with your SD-WAN Manager details:
   ```bash
   vi inventory/hosts.yml
   ```

3. Update device-specific variables:
   ```bash
   vi vars/device_vars.yml
   ```

4. Run the playbooks in sequence:
   ```bash
   # Exercise 1: Check for compatible device templates
   ansible-playbook -i inventory/hosts.yml playbooks/1_check_device_templates.yml
   
   # Exercise 2: List vSmart controllers
   ansible-playbook -i inventory/hosts.yml playbooks/2_list_vsmart_controllers.yml
   
   # Exercise 3: Attach template to vSmart controller
   ansible-playbook -i inventory/hosts.yml playbooks/3_attach_template.yml
   
   # Exercise 4: Verify attachment and sync status
   ansible-playbook -i inventory/hosts.yml playbooks/4_verify_attachment.yml
   ```

   Or run all exercises in sequence:
   ```bash
   ansible-playbook -i inventory/hosts.yml playbooks/complete_workflow.yml
   ```

## Repository Structure

```
.
├── LAB_GUIDE.md                      # Comprehensive lab guide with detailed instructions
├── README.md                          # This file
├── inventory/
│   └── hosts.yml                      # Ansible inventory with SD-WAN Manager details
├── vars/
│   └── device_vars.yml                # Device-specific variables for template attachment
└── playbooks/
    ├── 1_check_device_templates.yml   # Exercise 1: Check compatible templates
    ├── 2_list_vsmart_controllers.yml  # Exercise 2: List vSmart controllers
    ├── 3_attach_template.yml          # Exercise 3: Attach template to device
    ├── 4_verify_attachment.yml        # Exercise 4: Verify attachment and sync
    ├── complete_workflow.yml          # Run all exercises in sequence
    └── detach_template.yml            # Optional: Detach device from template
```

## Prerequisites

- Cisco SD-WAN Manager 20.15 accessible and configured
- Ansible 2.9 or later
- Python 3.6 or later
- Network connectivity to SD-WAN Manager
- Valid credentials with appropriate permissions

## Documentation

See [LAB_GUIDE.md](LAB_GUIDE.md) for:
- Detailed step-by-step instructions
- Environment setup
- Exercise walkthroughs
- Troubleshooting tips
- Best practices

## Security Note

The example inventory file contains default credentials for lab purposes only. For production use:

1. Use Ansible Vault to encrypt credentials:
   ```bash
   ansible-vault create inventory/vault.yml
   ```

2. Update playbooks to reference vault variables

3. Run playbooks with vault password:
   ```bash
   ansible-playbook -i inventory/hosts.yml playbooks/1_check_device_templates.yml --ask-vault-pass
   ```

## Support

For issues or questions related to:
- Cisco SD-WAN: [Cisco SD-WAN Documentation](https://www.cisco.com/c/en/us/support/routers/sd-wan/series.html)
- Ansible: [Ansible Documentation](https://docs.ansible.com/)

## License

This project is provided as-is for educational and lab purposes.