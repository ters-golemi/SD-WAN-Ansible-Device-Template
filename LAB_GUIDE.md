# Lab Guide: Managing Catalyst SD-WAN Devices Using Ansible

## Overview

This lab guide demonstrates how to manage Cisco Catalyst SD-WAN devices using Ansible with Cisco SD-WAN Manager 20.15. You will learn how to:

1. Check for compatible device templates for a vSmart controller
2. List vSmart controllers
3. Attach a vSmart controller with a compatible device template
4. Verify the vSmart controller is attached and in sync with the Device Template

## Prerequisites

- Cisco SD-WAN Manager 20.15 accessible and configured
- Ansible 2.9 or later installed
- Python 3.6 or later
- `cisco.catalystwan` Ansible collection or `cisco.sdwan` modules
- Network connectivity to SD-WAN Manager
- Valid credentials for SD-WAN Manager with appropriate permissions

## Lab Environment Setup

### Install Required Ansible Collections

```bash
ansible-galaxy collection install cisco.catalystwan
```

Or if using the older SDK:

```bash
pip install cisco-sdwan
```

### Configure Inventory

Create an inventory file `inventory/hosts.yml`:

```yaml
all:
  hosts:
    sdwan_manager:
      ansible_host: <sdwan-manager-ip>
      ansible_connection: local
  vars:
    vmanage_host: <sdwan-manager-ip>
    vmanage_port: 443
    vmanage_username: <username>
    vmanage_password: <password>
```

**Security Note**: For production environments, use Ansible Vault to encrypt sensitive credentials.

### Encrypt Credentials (Recommended)

```bash
ansible-vault create inventory/vault.yml
```

Add your credentials:

```yaml
vmanage_username: admin
vmanage_password: your_secure_password
```

## Lab Exercises

### Exercise 1: Check for Compatible Device Templates

This exercise demonstrates how to retrieve and list device templates that are compatible with vSmart controllers.

**Playbook: `playbooks/1_check_device_templates.yml`**

Run the playbook:

```bash
ansible-playbook -i inventory/hosts.yml playbooks/1_check_device_templates.yml
```

**Expected Output:**
- List of all device templates in the SD-WAN Manager
- Filtered list showing only vSmart-compatible templates
- Template names, device types, and IDs

**Verification:**
- Review the output to identify vSmart device templates
- Note the template name or ID for use in the next exercises

### Exercise 2: List vSmart Controllers

This exercise shows how to retrieve a list of all vSmart controllers registered in the SD-WAN Manager.

**Playbook: `playbooks/2_list_vsmart_controllers.yml`**

Run the playbook:

```bash
ansible-playbook -i inventory/hosts.yml playbooks/2_list_vsmart_controllers.yml
```

**Expected Output:**
- List of all vSmart controllers
- Device details including:
  - Device hostname
  - System IP
  - UUID
  - Site ID
  - Current status
  - Template attachment status

**Verification:**
- Identify the vSmart controller you want to attach to a template
- Note the device UUID or system IP for the next exercise

### Exercise 3: Attach vSmart Controller with Device Template

This exercise demonstrates how to attach a vSmart controller to a device template and provide device-specific variable values.

**Playbook: `playbooks/3_attach_template.yml`**

Before running this playbook:

1. Update the variables in `vars/device_vars.yml` with your specific values:
   - Template name (from Exercise 1)
   - Device UUID (from Exercise 2)
   - Device-specific variables required by the template

Run the playbook:

```bash
ansible-playbook -i inventory/hosts.yml playbooks/3_attach_template.yml
```

**Expected Output:**
- Confirmation of template attachment initiation
- Action ID or task ID for tracking the deployment
- Status of the attachment operation

**Verification:**
- Check that the playbook completes without errors
- Note the action/task ID for monitoring the deployment progress

### Exercise 4: Verify vSmart Controller Attachment and Sync Status

This exercise shows how to verify that the vSmart controller is successfully attached to the device template and is in sync.

**Playbook: `playbooks/4_verify_attachment.yml`**

Run the playbook:

```bash
ansible-playbook -i inventory/hosts.yml playbooks/4_verify_attachment.yml
```

**Expected Output:**
- Current template attachment status for the vSmart controller
- Configuration sync status
- Detailed device state information
- Confirmation that the device is in sync with the template

**Verification:**
- Confirm that `configOperationMode` shows "vmanage"
- Verify that `template` field shows the expected template name
- Check that the device status is "normal" or "reachable"

## Complete Lab Workflow

To run all exercises in sequence:

```bash
ansible-playbook -i inventory/hosts.yml playbooks/complete_workflow.yml
```

This playbook executes all four exercises in order, providing a complete demonstration of the device template management workflow.

## Troubleshooting

### Connection Issues

If you encounter connection errors:

1. Verify network connectivity to SD-WAN Manager:
   ```bash
   ping <sdwan-manager-ip>
   ```

2. Test HTTPS access:
   ```bash
   curl -k https://<sdwan-manager-ip>
   ```

3. Verify credentials are correct

### Authentication Failures

- Ensure the user account has appropriate permissions
- Check if the account is locked or password has expired
- Verify the vManage version matches the API version

### Template Attachment Issues

- Ensure all required template variables are provided
- Verify the device is in a valid state to accept template changes
- Check that the template is compatible with the device type
- Review SD-WAN Manager logs for detailed error messages

### Certificate Warnings

If using self-signed certificates, you may need to disable SSL verification:

```yaml
validate_certs: no
```

**Note**: Only use this in lab environments, not in production.

## Best Practices

1. **Use Ansible Vault** for storing credentials
2. **Test in a lab environment** before running in production
3. **Backup configurations** before making changes
4. **Monitor deployment progress** through SD-WAN Manager GUI or API
5. **Use version control** for your playbooks and templates
6. **Document device-specific variables** required for each template
7. **Implement error handling** in your playbooks
8. **Use tags** to run specific tasks within playbooks

## Additional Resources

- [Cisco SD-WAN Documentation](https://www.cisco.com/c/en/us/support/routers/sd-wan/series.html)
- [Ansible Documentation](https://docs.ansible.com/)
- [Cisco Catalyst WAN Ansible Collection](https://galaxy.ansible.com/cisco/catalystwan)

## Cleanup

To detach a device from a template:

```bash
ansible-playbook -i inventory/hosts.yml playbooks/detach_template.yml
```

**Warning**: Detaching a device from a template will remove the vManage-managed configuration and may disrupt service.

## Summary

In this lab, you have learned how to:

- Use Ansible to interact with Cisco SD-WAN Manager API
- Query and identify compatible device templates for vSmart controllers
- List and identify vSmart controllers in your SD-WAN fabric
- Attach a vSmart controller to a device template with specific variable values
- Verify successful template attachment and configuration synchronization

These skills form the foundation for automating SD-WAN device configuration management using Ansible.
