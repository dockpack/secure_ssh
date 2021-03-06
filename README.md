# Hardened SSH
[![Galaxy](https://img.shields.io/badge/galaxy-dockpack.secure__ssh-blue.svg?style=flat)](https://galaxy.ansible.com/dockpack/secure_ssh)
![Build Status](https://api.travis-ci.com/dockpack/secure_ssh.svg)

An Ansible role to harden SSH on Centos 7 with several options, like using a trusted CA.

This role is based on insights gained working with DISA-STIG, CIS, USG, NCSC, NIST, PCI, and other security norms. After scanning with openscap, I audited my server using the [SSH Observatory of Mozilla](https://observatory.mozilla.org), and finally with [ssh-audit](https://github.com/arthepsy/ssh-audit). I can still connect to Centos 7 with macOS Mojave.

I can run port forwarding, X11Forwarding, but an incredible amount of options is configurable.


## License: MIT



Key Management Requires Attention
---------------------------------

`distribute_ssh_keys: true`
In any larger organization, use of SSH key management solutions is almost
necessary. SSH keys should also be moved to root-owned locations with proper provisioning and termination processes. Users will not be able to modify their
pubkey because the immutable file attribute is set.

` AuthorizedKeysFile: '/etc/ssh/authorized_keys/%u'`


Hashicorp Vault
---------------
This role can be used to manage access to SSH by the means of signed ssh keys,
and to sftp with OTP.

Signed SSH keys
---------------

See SECURITY.md


To manage groups  without IAM, LDAP, AD.
----------------------------------------
`manage_ssh_groups: true` # Default is false.

Creates groups for various purposes.
```
ssh_groups:
  with_items:
    - wheel
    - staff
    - users
    - games
    - chroot
```

Manage users without IAM, LDAP, AD.
--------------------------------------
`manage_ssh_users: true` # Default is false.
Adds all users in 'ssh_users:' removes all 'non_users:'
Check the defaults/main.yml

```
ssh_users:
  - username: vagrant
    shell: /bin/bash
    group: wheel
    seuser: unconfined_u
```

# The `seuser` property of a user helps to confine users to policy classes
`semanage_ssh_users: true`

# There are five main SELinux seuser values:
- guest\_u: - no X windows, no sudo, and no networking
- xguest\_u: - same as guest\_u, but X and web connectivity is allowed
- user\_u: - same as xguest\_u, but networking isn’t restricted
- staff\_u: - same as user\_u, but sudo is allowed (su isn’t allowed)
- unconfined\_u: - full access


Fail2ban
--------

Some commands to verify the config. Hackers show up in /var/log/secure.

```
firewall-cmd --list-rich-rules
ipset list fail2ban-sshd
firewall-cmd --ipset=fail2ban-sshd --add-entry=222.186.52.124
ipset add fail2ban-sshd  112.85.42.237timeout 86400 -exist
fail2ban-client status sshd
```

45.55.176.164 The Mozilla SSH Observatory scans from sshscan.rubidus.com at 45.55.176.164.
