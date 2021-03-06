# Readme

## Role name

nar_compute_nodes_firmware_upgrade

## Description

For H-series compute nodes, NetApp provides the **nar_compute_nodes_firmware_upgrades** Ansible role that helps you automate firmware upgrade for hardware components, such as the BMC, BIOS, and NIC.

## Requirements

- The target systems should use RHEL.
- The NetApp HCI role variables should be set.

For details about recommended values, troubleshooting, caveats, and configurations for the environment, see  [here](README_USER_GUIDE.md).

```
- name: Compute Nodes FirmWare Upgrades
  hosts: all
  gather_facts: True
  roles:
    - role: nar_compute_nodes_firmware_upgrade
```

## Run Ansible playbooks

```
ansible-playbook -i hosts site.yml -e 'username=username password=password client-id=client-id audience=audience'
```

- When performing a cluster upgrade, provide `vcenter_ip` and `cluster_id` only.
- Do not set `controller_id` and `hardware_ids` through extended argument.
- Ensure that no values are specified for `controller_id` and `hardware_ids` in `all.yml`.
- When performing single-node upgrade, provide `controller_id` and `hardware_ids` only.
- Do not set `vcenter_ip` and `cluster_id` through extended argument.
- Ensure that no values are specified for `vcenter_ip` and `cluster_id` in `all.yml`.

## Example for running an individual task

When you execute a playbook, you can filter tasks based on tags in two ways:

--tags

--skip-tags

For more details, read [Tags](https://docs.ansible.com/ansible/2.9/user_guide/playbooks_tags.html).

Example:
```
ansible-playbook -i hosts site.yml -e 'username=username password=password client-id=client-id audience=audience' -t validate-requirements-prereqs
```

## Example for running a specific task

Create a `.yml` file within `ansible-roles/compute_nodes_firmware_upgrade/roles/nar_hci_compute_nodes_firmware_upgrade/tasks`.

Now add tasks to this file. For example: Filename: Validate_access_token.yml

```
---
- name: Hit /about API to collect mgmt bundle version, storage virtual ip and token url
  include_tasks: collect_mnode_about_response.yml
- name: Verify Admin access
  args:
    apply:
      delegate_to: localhost
  include_tasks: verify_admin_access_token.yml
```

## Playbook logs, messages, and recap

The standard output looks similar to the following:

```
PLAY [Hello World] *************************************************************

TASK [Say hello] ***************************************************************
ok: [127.0.0.1] => {
    "msg": "Hello, world!"
}

PLAY RECAP *********************************************************************
127.0.0.1                  : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
- Each task executes distinctly and can be read in the log messages.
- The collective information of the playbook result can be found in the `PLAY RECAP`.
- The Ansible `PLAY RECAP rescued` value is the count of nodes which failed upgrades, but were eventually rescued from aborting the playbook.
- The Ansible `PLAY RECAP failed` value if found displays the failed list of hardware nodes and/or the failed health checks details at the end of execution of the playbook.

## License

GNU v3

## Author Information

NetApp https://www.netapp.com
