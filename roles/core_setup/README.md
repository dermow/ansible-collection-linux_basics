dermow.linux_basics.core_setup
=========

Distribution independent core linux setup.


Role Variables
--------------
|  variable | type | default | description  |
|---|---|---|---|
| core_setup_users  | list | empty | list with users to create. See section 'Managing users and groups' |
| core_setup_groups  | list | empty | list with groups to create. See section 'Managing users and groups' |
| core_setup_auth_keys  | list | empty | list with authorized keys to add. See section 'Managing authorized keys' |
| core_setup_exclusive_key_management | boolean | false | If enabled, all keys not specified here for the user will be removed. BE CAREFUL! |
| core_setup_directories | list | empty | List of directoriesto be managed. See section 'Managing directories' |

Managing users and groups
--------------
You can use this role to manage one or more users and groups. Just set the variables 'core_setup_groups' and 'core_setup_users':
```yaml
- hosts: my_hosts
  tasks:
    - name: Include this role
      ansible.builtin.include_role:
        name: dermow.linux_basics.core_setup
      vars:
        core_setup_groups:
          - mygroup
        core_setup_users:
          name: myuser
          primary_group: mygroup
          additional_groups: []
          shell: "/bin/bash"
          state: present
          create_home: true
```

The following fields are available for each user item:

|  field | type | default | description  |
|---|---|---|---|
| name | string | none | name of the user |
| primary_group | string | none | name of the primary group for the user | 
| additional_groups | list | empty | list of additional groups for the user | 
| shell | string | /bin/bash | shell to set for the user. E.g. /bin/bash, /bin/zsh |
| state | ansible state | state for the user 'present' or 'absent' |
| create_home | boolean | true | should the home directory be created? |
| custom_home | string | none | should a custom home be set? If you do not want an custom home, do not specify that field |

Managing authorized keys
--------------
This role can be used to managed authorized keys for SSH access. 

```yaml
- hosts: my_hosts
  tasks:
    - name: Include this role
      ansible.builtin.include_role:
        name: dermow.linux_basics.core_setup
      vars:
        core_setup_auth_keys:
          - comment: mow
            user: mow
            key: ssh-rsa ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890
            state: present
```

Managing directories
--------------
This role can be used to managed directories on your servers. 

```yaml
- hosts: my_hosts
  tasks:
    - name: Include this role
      ansible.builtin.include_role:
        name: dermow.linux_basics.core_setup
      vars:
        core_setup_directories:
          - path: /opt/my_directory
            owner: mow
            group: ansible
            mode: "0660"
```

Example Playbook
----------------
```yaml
- name: Server setup
  hosts: all
  tasks:
    - name: Include core setup role
      ansible.builtin.include_role:
        name: core_setup
      vars:
        core_setup_groups:
          - "ansible"
        core_setup_users:
          - name: mow
            shell: "/bin/bash"
            primary_group: "ansible"
        core_setup_auth_keys:
          - comment: mow
            user: mow
            key: ssh-rsa ABCDEFGHIJKLMNOP
        core_setup_directories:
          - path: /opt/test
            owner: mow
            group: ansible
            mode: "0660"

```

License
-------
Apache License 2.0

Author Information
------------------
* Sebastian Seibold
* dermow@posteo.de
* https://github.com/dermow
