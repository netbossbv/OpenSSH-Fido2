# Setup-Yubikey-Fido2-with-persistant-ssh-key
This repo describes how to create a persistant Fido2 keypair for use with (Open)ssh

### ToDo:
- setup the Yubikey itself for use (set pin, puk and management key as weel as require touch for use)

### Prepare the Yubikey:
1. Insert a prepared Yubikey into your ssh client device, have Openssh version => 8.2 (preferable the latest version) and yubikey-manager installed:
2. ykman fido access change-pin
3. ykman fido access set-puk
4. ykman fido access set-touch --always
5. ykamn list (to get the serial(s) of your inserted Yubikey(s))

### Prepare the public key for use:
1. ssh-keygen -t ed25519-sk -O resident -f ~/.ssh/id_ed25519_sk_rk-xxxxxxxx (where xxxxxxxx is the serial of the Yubikey)
2. Add the contecnt of the .pub file to the ~/.ssh/authorized_keys file on the ssh host you want to log in to. You might need to ask the administrator to do that for you.

### Use your Yubikey on another device:
1. Make sure Openssh is of version 8.2 or higher (preferably the latest to have security issues fixed)
2. ssh-keygen -K and set no passphrase. This generates the file part refering to the private key stored in the secure element of the Yubikey and the file with the public key.
