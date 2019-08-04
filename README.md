# Hardened SSH

An Ansible role to harden SSH on Centos 7 with several options, like using a trusted CA.

This role is based on insights gained working with DISA-STIG, CIS, USG, NCSC, NIST, PCI, and other security norms.


## License: MIT

# There are five main SELinux seuser values:
- guest\_u: - no X windows, no sudo, and no networking
- xguest\_u: - same as guest\_u, but X and web connectivity is allowed
- user\_u: - same as xguest\_u, but networking isn’t restricted
- staff\_u: - same as user\_u, but sudo is allowed (su isn’t allowed)
- unconfined\_u: - full access
