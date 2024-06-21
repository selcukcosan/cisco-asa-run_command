# cisco-asa-run_command

This Ansible script runs arbitry commands on multiple Cisco ASA devices and writes the command outputs to the different files in output folder.

The inventory yaml format is simple as below.

```yml
---
all:
  children:
    all_cisco_asa:
      hosts:
        ciscoasa1:
          inventory_network_os: cisco.asa.asa
          inventory_network_cisco_asa_type: vpn
          inventory_network_cisco_asa_ha_type: standalone
          inventory_host: 192.168.1.98
          inventory_port: 22
          inventory_user: admin
          inventory_pass: password
    all_f5:
      hosts:
        bigip1:
          inventory_network_os: f5.bigip
          inventory_host: 192.168.1.245
          inventory_port: 443
          inventory_user: admin
          inventory_pass: password
        bigip2:
          inventory_network_os: f5.bigip
          inventory_host: 192.168.1.246
          inventory_port: 443
          inventory_user: admin
          inventory_pass: password
        bigip3:
          inventory_network_os: f5.bigip
          inventory_host: 192.168.1.247
          inventory_port: 443
          inventory_user: admin
          inventory_pass: password
```

## Prerequisite
Ansible ansible-pylibssh modules must be installed before running the script.
```bash
pip install ansible-pylibssh
```

## Usage:
```bash

ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -vvvv --vault-password-file vault_pass.yaml -i inventory-vault.yaml cisco-asa-run_command.yml --extra-vars="asa_device=ciscoasa1" --extra-vars="ha_type=standalone" --extra-vars="asa_type=vpn"
```

**_NOTE:_**  `output` folder must be created before running the script.

## Files
- cisco-asa-run_command.yml >> Ansible script file
- vault_pass.yaml >> Inventory vault password information
- inventory-vault.yaml >> Inventory vault file encrypted by "Inventory vault password"

## Variables
- `asa_device` variable shows which Cisco ASA device/devices will be connected.The value can be "ciscoasa1", "ciscoasa2" or "v" as per the example inventory file above.
- `ha_type` variable is just a tag for definition of Cisco ASA device. It can be "active", "standby", "standalone" etc. If you assign "any" value, the script will take any type of "ha_type"
- `asa_type` variable is just another tag variable. You can define the firewall types as "vpn", "internal", "external" etc. If you assign "any" value, the script will take any type of "asa_type"

## output Folder Example Files
- ciscoasa1-2024-06-21-18-02-16-show_version.txt 
