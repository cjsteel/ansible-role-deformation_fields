
ansible-role-deformation_fields
=======================

An Ansible role to install and manage **deformation_fields**


Description
-----------

Very Rough Draft

## Notes

You may want to remove / uninstall older versions before installing newer ones.


Resources
---------

-  https://deformation_fields.com/



Requirements
------------




Options
-------


Issues
------


Role Variables
--------------

### roles/deformation_fields/defaults/main.yml

```yaml
---
# file: roles/deformation_fields/defaults/main.yml
#

# Requirements
#
# set the value of project_deployment_user_name in your projects group_vars/all/defaults.yml
#
# ---
# file: group_vars/all/defaults.yml
#
# project_deployment_user_name: 'deployment_user_name'

deformation_fields_controller_home   : '{{ fact_controler_home }}'
deformation_fields_remote_user       : '{{ project_deployment_user_name }}'
deformation_fields_remote_users_home : '/home/users/{{ deformation_fields_remote_user }}'

# probably set in this or a dependent role

deformation_fields_state             : 'absent' # 'present' # 'absent'
#deformation_fields_installation_type : 'local' # 'url'
deformation_fields_app_name          : 'deformation_fields'
deformation_fields_package_name      : 'deformation_fields-desktop'
deformation_fields_ver               : '2.0.3' #'2.6.2' # 2.0.3
deformation_fields_arch              : 'amd64'
deformation_fields_package_type      : 'deb'

# calculated vars

# example below builds "deformation_fields-desktop-2.6.2-amd64.deb"
deformation_fields_package_filename  : '{{ deformation_fields_package_name }}-{{ deformation_fields_ver }}-{{ deformation_fields_arch }}.{{ deformation_fields_package_type }}'
deformation_fields_controller_package_path : '{{ fact_controller_home }}/src/Ubuntu/16.04/deformation_fields/{{ deformation_fields_ver }}/{{ deformation_fields_package_filename }}'

deformation_fields_taget_node_package_dir  : '{{ deformation_fields_remote_users_home }}/src/Ubuntu/16.04/deformation_fields/{{ deformation_fields_ver }}'
deformation_fields_taget_node_package_path : '{{ deformation_fields_taget_node_package_dir }}/{{ deformation_fields_package_filename }}'
```

### roles/deformation_fields/tests/vagrant.yml

```shell
---
# file: roles/{{ short_role_name }}/tests/vagrant.yml

- hosts: all
  remote_user: ubuntu
  become: false # or local directory creation will fail
  pre_tasks:

    - set_fact: fact_controller_user="{{ lookup('env','USER') }}"
    - debug: var=fact_controller_user

    - set_fact: fact_controller_home="{{ lookup('env','HOME') }}"
    - debug: var=fact_controller_home

  vars:

    - deformation_fields_controller_home   : '{{ fact_controler_home }}'
    - deformation_fields_remote_user       : 'ubuntu'
    - deformation_fields_remote_users_home : '/home/ubuntu'

# probably set in this or a dependent role

    - deformation_fields_state                   : 'absent' # 'present' # 'absent'
#    - deformation_fields_installation_type       : 'local' # 'url'
    - deformation_fields_app_name                : 'deformation_fields'
    - deformation_fields_package_name            : 'deformation_fields-desktop'
    - deformation_fields_ver                     : '2.0.3' #'2.6.2' # 2.0.3
    - deformation_fields_arch                    : 'amd64'
    - deformation_fields_package_type            : 'deb'
    # example below builds "deformation_fields-desktop-2.6.2-amd64.deb"
    - deformation_fields_package_filename        : '{{ deformation_fields_package_name }}-{{ deformation_fields_ver }}-{{ deformation_fields_arch }}.{{ deformation_fields_package_type }}'

    - deformation_fields_controller_package_path : '{{ fact_controller_home }}/src/Ubuntu/16.04/deformation_fields/{{ deformation_fields_ver }}/{{ deformation_fields_package_filename }}'

    - deformation_fields_taget_node_package_dir  : '{{ deformation_fields_remote_users_home }}/src/Ubuntu/16.04/deformation_fields/{{ deformation_fields_ver }}'
    - deformation_fields_taget_node_package_path : '{{ deformation_fields_taget_node_package_dir }}/{{ deformation_fields_package_filename }}'

  roles:
    - ../../
```



NOT IN USE AT THIS TIME - Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles. Examples:

* [ ansible-role-acemenu ]( https://github.com/cjsteel/ansible-role-acemenu )
* [ ansible-role-ensure_dirs ]( https://github.com/csteel/ansible-role-ensure_dirs )
* [ ansible-role-skel ]( https://github.com/csteel/ansible-role-skel )

### NOT IN USE AT THIS TIME  - ensure_dirs

#### NOT IN USE AT THIS TIME - roles/ansible-role-deformation_fields/meta/main.yml

```yaml
---
# file: roles/ansible-role-deformation_fields/meta/main.yml in dependant role
dependencies:

- { role: ensure_dirs, 
        ensure_dirs_on_remote: "{{ deformation_fields_remote_directories_description }}",
        ensure_dirs_on_local : "{{ deformation_fields_local_directories_description }}"
  }
```

#### NOT IN USE AT THIS TIME 

#### roles/ansible-role-deformation_fields/defaults/main.yml example

```yaml
deformation_fields_remote_directories_description:

  deformation_fields_installation_resources_dir:

    state       : "present"					# absent
    path        : "sys/sw"					# relative to Ansible users home
    owner       : "{{ ansible_ssh_user }}"
    group       : "{{ ansible_ssh_user }}"
    mode        : "0644"

deformation_fields_local_directories_description:

  deformation_fields_installation_resources_dir:

    state       : "present"					# absent
    path        : "sys/sw/" 				# relative to Ansible users home dir
    owner       : "{{ ansible_ssh_user }}"
    group       : "{{ ansible_ssh_user }}"
    mode        : "0644"
```



role playbook example
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:



### project_name/deformation_fields.yml

```yaml
---
# file: project_name/ants.yml

- hosts: deformation_fields
  become: false
  gather_facts: true
  pre_tasks:

    - set_fact: fact_controller_user="{{ lookup('env','USER') }}"
    - debug: var=fact_controller_user

    - set_fact: fact_controller_home="{{ lookup('env','HOME') }}"
    - debug: var=fact_controller_home

  roles:

    - { role: deformation_fields, deformation_fields_state: 'absent', deformation_fields_ver: '2.0.3' }
    - { role: deformation_fields, deformation_fields_state: 'present', deformation_fields_ver: '2.6.2' }

#    - { role: cjsteel.ansible-role-deformation_fields, deformation_fields_state: 'absent', deformation_fields_ver: '2.0.3' }
#    - { role: cjsteel.ansible-role-deformation_fields, deformation_fields_state: 'present', deformation_fields_ver: '2.6.2' }
```



## main playbook example

### project_name/systems.yml

```yaml
---
- hosts: all
  become: false

- include: deployment_user.yml

- include: shorewall.yml

- include: ldap.yml

- include: workstation.yml

- include: deformation_fields.yml
...
```



## Ansible command examples

### without sudo

```shell
ansible-playbook -i inventory/dev systems.yml --limit ace-ws-77
```

### with sudo

```shell
ansible-playbook -i inventory/dev systems.yml --limit ace-ws-77 --ask-become-pass
```



## Vagrant testing

```shell
mkdir -p .vagrant/synced
vagrant up
ssh-copy-id -p 2222 vagrant@localhost
vagrant ssh
```



## License

MIT

## Author Information

Christopher Steel on behalf of ACElabs  
Systems Administrator  
McGill Centre for Integrative Neuroscience  
Montreal Neurological Institute  
E-mail: christopherDOTsteel@mcgill.ca  
[mcin-cnim.ca](http://mcin-cnim.ca/)    
[theneuro.ca](http://www.mcgill.ca/neuro/)   

## Open Science

The Montreal Neurological Institute has adopted the principles of Open Science. We are inspired by the likes of the Allen Institute for Brain Science, the National Institutes of Health's Human Connectome project, and the Human Genome project. For additional information please see [open science at the neuro]( https://www.mcgill.ca/neuro/open-science-0).



![MCIN](imgs/mcin-logo-brain-140x116.png)          ![neuro](imgs/neuro-logo-160x116.png)  
