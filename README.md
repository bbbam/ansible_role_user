Ansible Role User
=========

This role installs sudo, creates a series of groups and the relevant users.

This role was built to be ran as part of the initial provisioning of servers.

Requirements
------------
If you are running this from a Mac, you need to install passlib.

`pip install passlib`.


Role Variables
--------------
Although there are some "sane" defaults, the following can be set.

* Users array
```users:
  - { username: user,
    groups: [sudo],
    shell: /bin/bash,
    set_password: true,
    home: /home/user,
    passwordless_sudo: false,
    authorized_keys: [~/.ssh/id_rsa.pub]
  }
  - { username: ansible,
    groups: [nopasswd_sudo],
    uid: 9999,
    shell: /bin/bash,
    home: /home/ansible,
    passwordless_sudo: true,
    authorized_keys: [~/.ssh/id_rsa.pub]}
```
Mind that some vars are optional (groups, uid, set_password).
Beware of `set_password`. If used, it will store the password in `/tmp/user_password` and you will be forced to change the password during the first login. If you omit it, it will lock the password (`usermod -L`). You prefer the latter for deployment accounts and nopasswd sudo.
* sudoers
```
sudoers_groups:
  - { name: "sudo", perms: "ALL=(ALL:ALL) ALL" }
  - { name: "nopasswd_sudo", perms: "ALL=(ALL:ALL) NOPASSWD:ALL" }
```
Creates the relevant sudoers group.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: frite.user_creation }

Installation is as easy as `ansible-galaxy install frite.user_creation`

Contribution guidelines
----------------------

Issues are welcome and so are code contributions.
Reg. code contributions, your code needs to pass all tests,
i.e. `molecule test` must succeed.

License
-------

MIT
