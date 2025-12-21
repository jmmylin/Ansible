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

# 12/20/25
I was able to setup my EVE-NG lab. Get R1 setup with SSH. Verify that I can SSH directly to the router. I installed Ansible onto my Ubuntu computer and made a simple Ansible call to R1.

## Setting up Ansible on my Ubuntu laptop and connecting to my EVE-NG lab
```bash
ansible-playbook -i inventory.yaml network_config.yaml --ask-pass --ask-become-pass
```
Note currently there is not an enable secret create on the R1.

## SSH stuff
```bash
ansible-playbook -i inventory.yaml network_config.yaml \
  --ssh-common-args="-o KexAlgorithms=diffie-hellman-group14-sha1 -o HostkeyAlgorithms=+ssh-rsa -o PubkeyAcceptedAlgorithms=+ssh-rsa"
```

## Test Ansible connection
```bash
ansible -i inventory.yaml R1 -m ansible.netcommon.cli_command -a "command='show version'" --ask-pass
ansible-playbook -i inventory.yaml network_config.yaml --limit R1 -t "Gather device facts"
ansible-playbook -i inventory.yaml network_config.yaml
```

## Turn off deprecation warnings
I created an ansible.cfg file in my folder with my playbook and inventory. 
```ini
[defaults]
deprecation_warnings = False
```

## host_vars and group_vars
```bash
ansible-playbook -i inventory.yaml -k network_config_rev1.yaml
```
Ansible will override variables found in the host_vars folder with any conflicting variables in the group_vars.
Notice that there is a loop that is calling the variable that I defined in the group_vars and host_vars files. The variable is a list and is stored in a loop variable called item. Just like a for loop in python etc. Specific parts are called from that variable list.
```yaml
  - name: Configure Interface Settings (descriptions, enabled)
      cisco.ios.ios_interfaces:
        config:
          - name: "{{ item.name }}"
            description: "{{ item.description | default(omit) }}"
            enabled: "{{ item.enabled | default(omit) }}"
        state: merged
      loop: "{{ interface_configs }}"
```
