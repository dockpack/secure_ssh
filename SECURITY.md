# Security Policy

- Make sure you have enabled [Two-factor Authentication for GitHub](https://github.com/settings/security).
- Enable [Signed Commits](https://help.github.com/en/articles/signing-commits) for Git.

## Secure Shell

Generate a strong SSH keypair with a strong passphrase, and store the passphrase in your Keychain. 
This means that your SSH private key is safely secured in your Mac's Keychain, plus you will never need to type the passphrase.

```sh
ssh-keygen -t rsa -b 4096
ssh-agent bash
ssh-add -K
```

Add a config file to the .ssh directory hidden in your home directory with the following content:

```text
IgnoreUnknown UseKeychain
Host *
  UseKeychain yes
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_rsa
```

To do this open Terminal and enter:

```sh
open -a textedit ~/.ssh/config
```

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
