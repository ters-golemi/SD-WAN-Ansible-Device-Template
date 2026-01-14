# Contributing to SD-WAN Ansible Device Template Management

Thank you for your interest in contributing to this project! This guide will help you understand how to extend and enhance the lab materials.

## Ways to Contribute

- Add support for additional device types (vEdge, vBond, vManage)
- Enhance error handling in playbooks
- Add more comprehensive test scenarios
- Improve documentation and examples
- Add support for different SD-WAN Manager versions
- Create additional use cases and workflows

## Development Guidelines

### Code Style

- Follow Ansible best practices
- Use meaningful variable names
- Add comments to explain complex logic
- Keep playbooks modular and reusable
- Use YAML anchors and aliases where appropriate

### Playbook Structure

Each playbook should follow this structure:

```yaml
---
# Playbook: Description
# Additional context about what this playbook does

- name: Descriptive Playbook Name
  hosts: sdwan_manager
  connection: local
  gather_facts: no
  vars:
    ansible_python_interpreter: /usr/bin/python3
  
  tasks:
    - name: Clear, descriptive task name
      # Task implementation
```

### Testing

Before submitting changes:

1. Validate YAML syntax:
   ```bash
   python3 -c "import yaml; yaml.safe_load(open('playbooks/your_playbook.yml'))"
   ```

2. Run Ansible syntax check:
   ```bash
   ansible-playbook --syntax-check playbooks/your_playbook.yml -i inventory/hosts.yml
   ```

3. Test against a lab environment if possible

### Documentation

- Update README.md if adding new features
- Add detailed comments in playbooks
- Update LAB_GUIDE.md with new exercises
- Include examples in QUICK_REFERENCE.md
- Document all variables in vars/device_vars.yml

## Adding New Device Types

To add support for a new device type (e.g., vEdge):

1. Create playbooks following the naming convention:
   - `1_check_vedge_templates.yml`
   - `2_list_vedge_devices.yml`
   - `3_attach_vedge_template.yml`
   - `4_verify_vedge_attachment.yml`

2. Update device filter in playbooks:
   ```yaml
   - name: Filter vEdge devices
     set_fact:
       vedge_devices: "{{ all_devices.json.data | selectattr('device-type', 'equalto', 'vedge') | list }}"
   ```

3. Add device-specific variables to `vars/`

4. Update documentation

## Adding New Features

When adding new features:

1. Create a new branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. Make your changes following the guidelines

3. Test thoroughly

4. Update documentation

5. Submit a pull request with:
   - Clear description of the feature
   - Why it's useful
   - How to test it
   - Any breaking changes

## Example Contributions

### Adding vEdge Support

1. Create `playbooks/vedge/1_check_vedge_templates.yml`
2. Add vEdge-specific variables to `vars/vedge_vars.yml`
3. Update `LAB_GUIDE.md` with vEdge exercises
4. Test with actual vEdge devices

### Adding Retry Logic

```yaml
- name: Attach device to template with retry
  uri:
    url: "https://{{ vmanage_host }}:{{ vmanage_port }}/dataservice/template/device/config/attachfeature"
    method: POST
    body: "{{ attachment_payload | to_json }}"
    # ... other parameters
  register: attach_response
  retries: 3
  delay: 10
  until: attach_response.status == 200
```

### Adding Idempotency Check

```yaml
- name: Check if device already attached to template
  set_fact:
    already_attached: "{{ target_device.template | default('') == template_name }}"

- name: Skip attachment if already attached
  debug:
    msg: "Device already attached to {{ template_name }}"
  when: already_attached

- name: Attach device to template
  # ... attachment tasks
  when: not already_attached
```

## Security Considerations

- Never commit credentials or sensitive data
- Use Ansible Vault for all examples with credentials
- Validate all user inputs
- Follow principle of least privilege
- Document security best practices

## Code Review Checklist

Before submitting:

- [ ] YAML syntax is valid
- [ ] Playbooks pass ansible-playbook --syntax-check
- [ ] Documentation is updated
- [ ] No hardcoded credentials
- [ ] Error handling is implemented
- [ ] Code follows existing patterns
- [ ] Variables are well-documented
- [ ] Examples are provided

## Getting Help

If you need help:

1. Check existing documentation
2. Review similar playbooks in the repository
3. Consult Ansible documentation
4. Open an issue for discussion

## License

By contributing, you agree that your contributions will be licensed under the same license as the project.

## Thank You

Your contributions help make this lab better for everyone learning SD-WAN automation with Ansible!
