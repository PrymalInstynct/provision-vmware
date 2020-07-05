Provision VMWare
=========

Role to help with manageing Virtual Machines in a Dev Environment

	ansible-playbook provision-vmware.yml --tags create_new

You will be prompted to enter a list of VM Names this list can be single of comma-seperated

Requirements
------------

	- PyVmomi

Role Variables
--------------

| Variable | Default | Description |
|----------|---------|-------------|
| `vmware_name` | 10.10.1.20 | vCenter or ESXi IP Address or Hostname |
| `vmware_user` | root | User with rights to VMWare Assests |
| `vmware_pass` | `vault_vmware_pass` | Set password for VMWare user in group_vars/all/vault.yml |

Tags
----

| Tags | Description |
|------|-------------|
| `create_new` | Create a New VM with an ISO mounted |
| `create_from_template` | Create a New VM from an OFV Template found in files/* |
| `delete` | Delete a VM along with cleaning up the ansible host file |
| `snapshot` | Take a snapshot of the given VM |

Example Playbook
----------------

	- hosts: localhost
	  connection: local
	  gather_facts: no
	
	  vars_prompt:
	    - name: guest_name_list
	      prompt: Specify the VM name, comma seperated?
	      private: no
	
	  pre_tasks:
	    - name: Split VM names into variable loop
	      set_fact:
	        guest_name: "{{ guest_name_list.split(',') }}"
	      tags:
	        - always
	
	  roles:
	    - role: provision-vmware

