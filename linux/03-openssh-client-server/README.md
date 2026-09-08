# OpenSSH Client & Server Configuration

## Overview

Hands-on OpenSSH lab completed as part of LFCS preparation.

This lab covers:

* SSH key pair generation
* Ed25519 authentication
* SSH public-key deployment
* OpenSSH server hardening
* Disabling direct root SSH login
* Disabling password-based SSH authentication
* Validating `sshd_config`
* Reloading the SSH service without terminating existing sessions
* Testing and verifying key-based authentication

> Hostnames, usernames, IP addresses, key fingerprints, and other environment-specific details are sanitized for public documentation.

---

# Objectives

## Client

* Generate an Ed25519 SSH key pair
* Ensure existing SSH keys are not overwritten
* Deploy the public key to the target account
* Authenticate using the SSH key without entering the target account password

## Server

* Disable direct root SSH login
* Disable password authentication
* Validate the SSH daemon configuration before applying changes
* Reload the SSH daemon without terminating existing sessions

## Verification

* Verify passwordless key-based authentication
* Verify that password authentication is rejected
* Verify that direct root login is rejected

---

# Task 3.1 — OpenSSH Client Configuration

**Documented Client:** `linux-node01`

**Documented Target:** `linux-node02`

**Documented Target IP:** `192.168.10.12`

## 1. Prepare the SSH Directory

The client user did not initially have an `.ssh` directory.

Create it:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

The SSH directory should be owned by the user performing the SSH operations.

---

## 2. Generate an Ed25519 Key Pair

Generate an Ed25519 key pair:

```bash
ssh-keygen -t ed25519
```

When prompted for the file location, a custom filename can be used to avoid overwriting an existing key:

```text
~/.ssh/id_ed25519_lfcs
```

This is useful when an existing `id_ed25519` or another key already exists.

Verify the generated files:

```bash
ls -l ~/.ssh/
```

The key pair consists of:

```text
Private key → ~/.ssh/id_ed25519_lfcs
Public key  → ~/.ssh/id_ed25519_lfcs.pub
```

> Never publish a private SSH key. Only the public key may be shared with the target server.

---

## 3. Copy the Public Key to the Server

The public key can be transferred using `ssh-copy-id`:

```bash
ssh-copy-id -i ~/.ssh/id_ed25519_lfcs.pub adminuser@192.168.10.12
```

The target account password is required during the initial key installation.

After the key has been installed, connect using the corresponding private key:

```bash
ssh -i ~/.ssh/id_ed25519_lfcs adminuser@192.168.10.12
```

Once public-key authentication is working, the target account password is no longer required for SSH authentication.

> If the private key uses the default filename, SSH can normally locate it automatically. When using a custom filename, specify it with `-i` or configure it in `~/.ssh/config`.

---

# Task 3.2 — OpenSSH Server Hardening

**Documented Server:** `linux-node02`

## 1. Edit the SSH Daemon Configuration

Edit:

```bash
sudo vim /etc/ssh/sshd_config
```

Configure:

```text
PermitRootLogin no
PasswordAuthentication no
```

These settings:

* Prevent direct SSH login as `root`
* Disable password-based SSH authentication

Public-key authentication remains available.

---

## 2. Validate the Configuration

Before reloading the SSH daemon, validate the configuration syntax:

```bash
sudo sshd -t
```

A successful validation produces no output and returns a successful exit status.

This check helps prevent applying an invalid SSH configuration.

---

## 3. Apply the Configuration

Reload the SSH daemon:

```bash
sudo systemctl reload sshd
```

Reloading the service applies the new configuration without terminating existing SSH sessions.

Verify the service:

```bash
sudo systemctl status sshd
```

---

# Verification

## 1. Verify Key-Based Authentication

From the client:

```bash
ssh adminuser@192.168.10.12
```

Successful login without being prompted for the target account password confirms that public-key authentication is working.

---

## 2. Test Password Authentication Explicitly

To test whether public-key authentication is being used, disable it for a single SSH attempt:

```bash
ssh -o PubkeyAuthentication=no adminuser@192.168.10.12
```

Expected result:

```text
Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```

This confirms that password authentication is not being offered by the server and that authentication is restricted to the remaining enabled methods.

---

## 3. Test Root Login

Attempt a direct root login:

```bash
ssh root@192.168.10.12
```

Expected result:

```text
Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```

This confirms that direct root SSH login is disabled.

---

# Troubleshooting

## SSH Key Directory Did Not Exist

### Symptom

Running:

```bash
ssh-keygen -t ed25519
```

initially failed because the user's `.ssh` directory did not exist.

### Resolution

The directory was created and secured:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

The key pair was then generated successfully.

---

## Understanding `ssh-copy-id`

`ssh-copy-id` is used to append a client's public key to the target user's:

```text
~/.ssh/authorized_keys
```

It normally requires the target account's password during the initial installation.

After the key is installed, subsequent SSH authentication can use the private key instead of the account password.

---

# Key Learnings

* Learned how to generate an Ed25519 SSH key pair.
* Learned that the SSH `.ssh` directory must exist with appropriate permissions before storing user SSH keys.
* Learned how `ssh-copy-id` installs a public key into the target user's `authorized_keys`.
* Learned the difference between the private key and public key and why the private key must never be published.
* Learned how to avoid overwriting an existing SSH key by specifying a different key filename.
* Learned how to disable direct root SSH access with:

  ```text
  PermitRootLogin no
  ```
* Learned how to disable password-based SSH authentication with:

  ```text
  PasswordAuthentication no
  ```
* Learned to validate the SSH daemon configuration before applying changes:

  ```bash
  sshd -t
  ```
* Learned that `systemctl reload sshd` applies SSH configuration changes without terminating existing SSH sessions.
* Learned that:

  ```bash
  ssh -o PubkeyAuthentication=no <user>@<server>
  ```

  can be used to disable public-key authentication for a specific SSH connection and test the server's other authentication methods.
* Reinforced the importance of testing authentication changes before closing an existing administrative SSH session.

---

# Useful Commands

### Generate an Ed25519 key

```bash
ssh-keygen -t ed25519
```

### Copy a public key

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub <user>@<server>
```

### Test SSH using a specific private key

```bash
ssh -i ~/.ssh/<private-key> <user>@<server>
```

### Validate `sshd_config`

```bash
sudo sshd -t
```

### Reload OpenSSH

```bash
sudo systemctl reload sshd
```

### Disable public-key authentication for one connection

```bash
ssh -o PubkeyAuthentication=no <user>@<server>
```

### Check SSH service status

```bash
sudo systemctl status sshd
```

---

# Result

The OpenSSH client and server configuration was completed successfully.

The lab demonstrated:

* Ed25519 key-based authentication
* Public-key deployment
* SSH server hardening
* Root login restriction
* Password authentication restriction
* Configuration validation
* Safe service reloads
* Authentication troubleshooting and verification
