# Ansible Notes

## Run the playbook:
```bash
cd ~/Documents/ANSIBLE
ansible-playbook -i inventory.yaml network_config.yaml
```

- This playbook covers the core of topic 1.1. Practice in a lab (e.g., Cisco CML or EVE-NG) to see idempotency in action.



## Notes
- Idempotency Testing: Use state: merged (adds/updates) or state: replaced (overwrites entire resource).
- Error Handling: Add ignore_errors: true for gathering facts if some devices fail.
- Validation: Use ios_command to run show run before/after for verification.
- Advanced: For multi-device consistency, use roles (directory structure) and Jinja2 templates for dynamic configs.
- Asset Management: Gathering facts (as above) helps track hardware/software inventory—often integrated with tools like Cisco DNA Center or external CMDBs.
