# Security Policy

- Make sure you have enabled [Two-factor Authentication for GitHub](https://github.com/settings/security).
- Enable [Signed Commits](https://help.github.com/en/articles/signing-commits) for Git.

## Better Secure Shell Key Pairs

Generate a strong SSH keypair with a strong passphrase and bruteforce protection, and store the passphrase in your Keychain. This means that your SSH private key is safely secured in your Mac's Keychain, plus you will never need to type the passphrase. The -a 500 option adds 500 rounds of juggling which takes several seconds for each decryption attempt; no problem if you use the ssh-agent, but a huge problem for brute-forcing the passphrase!


```sh
ssh-keygen -a 500 -t ed25519 -C $USER

### macOS

```sh
ssh-add -K
```

### Linux

```sh
eval $(ssh-agent bash)
ssh-add
```

Add a config file to the .ssh directory hidden in your home directory with the following content:

```text
HashKnownHosts yes
UseKeychain yes
XAuthLocation /opt/X11/bin/xauth

Host *
  UseKeychain yes
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
```

To do this open Terminal and enter:

```sh
open -a textedit ~/.ssh/config
```

## CA Signed SSH keys

When you want to manage user's access out-of-bound, then you don't want to
install their public key in authorized_keys files on the servers they need to access. You can create a Certificate Signing Authority for Secure Shell (different from X509 TLS/SSL) by creating an ssh keypair. The public key is installed on all servers, and that is the only file that needs to be there. (You can have 2 CA keys or more if you need.)

```
ssh-keygen -t rsa -b 4096 -f ~/.ssh/ca_ssh
cat ~/.ssh/ca_ssh.pub >> playbook_dir/files/ssh/trusted-user-ca-keys.pub
```

This role copies the file to the servers and adds it to **/etc/ssh/sshd_config**:
`TrustedUserCAKeys /etc/ssh/trusted-user-ca-keys.pub`


# Granting Access
To sign Fred's public key (fred.pub) to grant one week access to sftp://download@server and log as customer in /var/log/secure, run this
command and send the fred.pub-cert file back to Fred.

```
ssh-keygen -s ~/.ssh/ca_ssh -I customer -n download -V +1w fred.pub
```

# Revoking keys
Add public keys of people that should no longer login to:
- playbook_dir/files/ssh/revoked\_keys


# Passwords are bad.

We don't use them, but if you do, then read along...

### General password tips

- Install a Password manager. Make sure the Vault (encrypted database storing your logins) is syncing to a second location.
- You must prevent getting into a situation where you can't login to a seldom-used server in case of emergency because you can't find the password;
- You must not use easy passwords to prevent brute force attacks, so it is best to have your Password Manager fill out the computer-generated passwords in login forms;
- You must not use the same password twice, so multiple layers of defence (first VPN, then a login password) actually improve security;
- You must prevent being locked out of one of your accounts because you forgot your password after a long holiday;
- You must prevent not being able to restore access to important accounts because you have lost the 2FA recovery codes and didn't have a backup.

Related reading: [Sam Rueby: I don’t know any of my passwords and neither should you](https://samrueby.com/2014/10/07/i-dont-know-any-of-my-passwords-and-neither-should-you/)
