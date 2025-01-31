# OpenSSH-Fido2

This repo describes how to create a persistant Fido2 keypair for use with (Open)ssh

### ToDo:
- setup the Yubikey itself for use (set pin, puk and management key as weel as require touch for use)

### Prerequisites
- Client side OS: Any OS that supports both the Yubikey Manager software and OpenSSH. So most common Linux distro's, MacOS from version 12 and up
  and Windows 11 will do.
  - NOTE on Windows 11: Be aware of the GUI integration MicroSoft complied in OpenSSH with. Pop-ups for touching you Yubikey or entering the PIN
  might appear behind your terminal window....
  - NOTE on MacOS: Replace the default OpenSSH with a HomeBrew installed version. That's more up-to-date and free of Apple's choices of what
  functions are good or bad for you and thus compiled without esseantials now and then.
- Server side OS: Any OS that can run OpenSSH. OpenBSD is a good choice since that is the first OS up-to-date with OpenSSH since that is maintaned
  within the OpenBSD Project.
- OpenSSH on both client and server version => 8.2 but not 8.5 or 9.7!!! See:
  - https://security-tracker.debian.org/tracker/CVE-2024-6387
  - https://www.openwall.com/lists/oss-security/2024/07/01/1
  The latest version is always best.
- Yubikey Manager CLI: https://github.com/Yubico/yubikey-manager/releases
- One or more Yubikey (One is none! If it breaks or get lost. Always prepare a backup device) with firmware version => 5.2.3.

### Prepare the Yubikey:
1. Insert a prepared Yubikey into your ssh client device, have Openssh version => 8.2 (preferable the latest version) and yubikey-manager installed:
2. ykman fido access change-pin
3. ykman list (to get the serial(s) of your inserted Yubikey(s))

### Prepare the public key for use:
1. ssh-keygen -t ed25519-sk -O resident -O verify-required C "xxxxxxxx" (where xxxxxxxx is the serial of the Yubikey)
2. Add the content of the .pub file to the ~/.ssh/authorized_keys file on the ssh host you want to log in to. You might need to ask the administrator to do that for you.

### Use your Yubikey on another device:
1. Make sure Openssh is of version 8.2 or higher (preferably the latest to have security issues fixed)
2. ssh-keygen -K and set no passphrase. This generates the file part refering to the private key stored in the secure element of the Yubikey and the file with the public key.
