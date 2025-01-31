# OpenSSH-Fido2

This repo describes how to create a persistant Fido2 keypair for use with (Open)ssh

### ToDo:
- Add SSHD config
- Add SSH config

### 1. Prerequisites
1. Client side OS: Any OS that supports both the Yubikey Manager software and OpenSSH. Most common Linux distro's, MacOS from version 12 and up
  and Windows 11 will do.
  a. NOTE on Windows 11: Be aware of the GUI integration MicroSoft complied in OpenSSH with. Pop-ups for touching you Yubikey or entering the PIN
  might appear behind your terminal window....
  b. NOTE on MacOS: Replace the default OpenSSH with a HomeBrew installed version. That's more up-to-date and free of Apple's choices of what
  functions are good or bad for you and thus compiled without esseantials now and then.
2. Server side OS: Any OS that can run OpenSSH. OpenBSD is a good choice because OpenSSH is maintaned within the OpenBSD Projects and is thus the
  first OS having the latest update.
3. OpenSSH on both client and server version => 8.2 but not 8.5 or 9.7!!! See:
  a. https://security-tracker.debian.org/tracker/CVE-2024-6387
  b. https://www.openwall.com/lists/oss-security/2024/07/01/1
  The latest version is always best.
4. Yubikey Manager CLI: https://github.com/Yubico/yubikey-manager/releases
5. One or more Yubikey(s) with firmware version => 5.2.3 (One is DANGEROUS in case it breaks or gets lost. Always prepare a backup device).

### 2. Prepare the Yubikey:
1. Insert a Yubikey into your ssh client device with Openssh version => 8.2 and yubikey-manager latest installed.
2. 'ykman fido access change-pin' to set the pin-code of the fido2 module
3. Optional: ykman fido access set-min-lenght (set minimum lenght of the pin to 6 digits)
4. 'ykman list' (to get the serial(s) of your inserted Yubikey(s))

### 3. Prepare the public key for use:
1. ssh-keygen -t ed25519-sk -O resident -O verify-required C "xxxxxxxx" (where xxxxxxxx is the serial of the Yubikey)
2. Add the content of the .pub file to the ~/.ssh/authorized_keys file on the ssh host you want to log in to. You might need to ask the administrator to do that for you.

### 4. Use the Yubikey:
1. Example ssh command: 'ssh -i ~/.ssh/id_ed25519_sk_xxxxxxxx user@sshserver.ext' where:
  a. '-i' sets the Identity file to the file that refers to the private key in the corresponding Yubikey. xxxxxxxx Refers to the serial of your Yubikey.
2. This can be automated and extended with back-up Yubikeys in your ~/.ssh/config or /etc/ssh/config (systemwide) files or a combination.

### 5. Use your Yubikey on another device:
1. Make sure Openssh is of version 8.2 or higher (preferably the latest to have security issues fixed)
2. 'ssh-keygen -K' and set no passphrase. This generates the file part refering to the private key stored in the secure element of the Yubikey and the file with the public key.
3. See 4. to use the Yubikey for authenticating so your SSH server.
