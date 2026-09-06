# Linux Networking, Hostname & Local Resolution

## Overview

Hands-on networking lab completed as part of LFCS preparation.

The lab practices hostname management, static IPv4 addressing, secondary IPv4 addresses, DNS configuration, IPv6 configuration, local hostname resolution, and network troubleshooting across Ubuntu and AlmaLinux.

Ubuntu uses **Netplan**, while AlmaLinux uses **NetworkManager (`nmcli`)**.

> Hostnames, IP addresses, and other environment-specific values in this document are sanitized for public documentation.

---

# Objectives

## Ubuntu

* Configure a persistent hostname
* Configure static IPv4 addressing using Netplan
* Add a secondary IPv4 address
* Configure DNS nameservers
* Safely test Netplan changes
* Permanently apply the configuration
* Configure local hostname resolution using `/etc/hosts`

## AlmaLinux

* Configure a persistent hostname
* Inspect the active NetworkManager connection
* Configure static IPv4 addressing using `nmcli`
* Add a secondary IPv4 address
* Configure DNS nameservers
* Disable IPv6
* Reactivate the connection
* Configure local hostname resolution using `/etc/hosts`

---

# Task 1.1 — Ubuntu

**Documented Host:** `ubuntu-node01`

## 1. Configure Hostname

Set the hostname:

```bash
sudo hostnamectl set-hostname ubuntu-node01.lfcs.lab
```

Verify:

```bash
hostnamectl
hostname
```

The hostname change takes effect immediately and persists across reboots.

---

## 2. Inspect Netplan

List the Netplan configuration files:

```bash
ls -l /etc/netplan/
```

Review the applicable configuration:

```bash
sudo cat /etc/netplan/<configuration-file>.yaml
```

The documented example configuration uses:

| Setting        | Example Value        |
| -------------- | -------------------- |
| Interface      | `ens33`              |
| Primary IPv4   | `192.168.10.11/24`   |
| Secondary IPv4 | `192.168.10.21/24`   |
| Gateway        | `192.168.10.1`       |
| DNS            | `1.1.1.1`, `8.8.8.8` |

> These are sanitized documentation values.

After editing the Netplan configuration, test it safely:

```bash
sudo netplan try
```

After confirming connectivity, apply the configuration:

```bash
sudo netplan apply
```

### Verification

Check addressing:

```bash
ip addr show ens33
```

Check routing:

```bash
ip route
```

Check DNS:

```bash
resolvectl status
```

---

## 3. Configure Local Host Resolution

Add the peer system to `/etc/hosts`:

```text
192.168.10.12 peer-node.internal
```

Verify:

```bash
getent hosts peer-node.internal
```

---

# Task 1.2 — AlmaLinux

**Documented Host:** `almalinux-node02`

## 1. Configure Hostname

Set the hostname:

```bash
sudo hostnamectl set-hostname almalinux-node02.lfcs.lab
```

Verify:

```bash
hostnamectl
hostname
```

---

## 2. Inspect NetworkManager

Check available network devices:

```bash
nmcli device status
```

List connection profiles:

```bash
nmcli connection show
```

Identify the active connection associated with the Ethernet interface before modifying it.

---

## 3. Configure IPv4, DNS and IPv6

The documented example configuration uses:

| Setting        | Example Value        |
| -------------- | -------------------- |
| Interface      | `ens160`             |
| Primary IPv4   | `192.168.10.12/24`   |
| Secondary IPv4 | `192.168.10.22/24`   |
| Gateway        | `192.168.10.1`       |
| DNS            | `8.8.8.8`, `1.1.1.1` |
| IPv6           | Disabled             |

### Configure the connection

Use the actual connection profile identified with:

```bash
nmcli connection show
```

A secondary IPv4 address can be added using:

```bash
sudo nmcli connection modify "<connection-name>" \
  +ipv4.addresses "192.168.10.22/24"
```

IPv4, gateway, DNS, and IPv6 can be configured using:

```bash
sudo nmcli connection modify "<connection-name>" \
  ipv4.method manual \
  ipv4.addresses "192.168.10.12/24" \
  ipv4.gateway "192.168.10.1" \
  ipv4.dns "8.8.8.8,1.1.1.1" \
  ipv6.method disabled
```

Reactivate the connection:

```bash
sudo nmcli connection up "<connection-name>"
```

### Verification

Check the interface:

```bash
ip addr show ens160
```

Check routing:

```bash
ip route
```

Check NetworkManager state:

```bash
nmcli device status
```

Review the connection profile:

```bash
nmcli connection show "<connection-name>"
```

---

## 4. Configure Local Host Resolution

Add the Ubuntu peer to `/etc/hosts`:

```text
192.168.10.11 peer-node.internal
```

Verify:

```bash
getent hosts peer-node.internal
```

---

# Troubleshooting

## Accidentally Deleted the NetworkManager Connection

During the AlmaLinux configuration, an incorrect `nmcli` command was executed while attempting to modify the network configuration.

The command was intended to modify IPv6 settings but instead resulted in the relevant NetworkManager connection profile being deleted.

### Recovery

The Ethernet device was reconnected using:

```bash
nmcli device connect <ethernet-device>
```

The network configuration was then rebuilt and the required IPv4, DNS, and IPv6 settings were applied again.

The task was subsequently completed and verified successfully.

### Lesson

Commands that operate on NetworkManager connection profiles must be used carefully.

Before modifying a connection, inspect the available profiles:

```bash
nmcli connection show
```

and check device state:

```bash
nmcli device status
```

Use:

```bash
nmcli connection modify
```

when changing an existing connection.

Be especially careful with:

```bash
nmcli connection delete
```

because it removes a connection profile.

---

# Key Learnings

* Learned how to persistently change a Linux hostname using `hostnamectl`.
* Practiced Ubuntu network configuration using Netplan.
* Practiced AlmaLinux network configuration using NetworkManager and `nmcli`.
* Learned how to add a secondary IPv4 address using the `+ipv4.addresses` property.
* Learned how to disable IPv6 completely for a NetworkManager connection.
* Learned how to recover a network device after accidentally deleting its NetworkManager connection profile using:

  ```bash
  nmcli device connect <ethernet-device>
  ```
* Reinforced the difference between **modifying** a connection and **deleting** a connection profile.
* Practiced local hostname resolution using `/etc/hosts`.
* Learned to use system documentation to find configuration and command syntax:

  ```bash
  man 7 netplan
  man nmcli-examples
  ```
* Reinforced the importance of inspecting existing network configuration before making changes, especially when working through a remote session.

---

# Verification Summary

## Ubuntu

```bash
hostnamectl
ip addr show ens33
ip route
resolvectl status
getent hosts peer-node.internal
```

Expected documented example:

```text
Hostname:       ubuntu-node01.lfcs.lab
Primary IPv4:   192.168.10.11/24
Secondary IPv4: 192.168.10.21/24
Gateway:        192.168.10.1
Peer:           192.168.10.12
```

## AlmaLinux

```bash
hostnamectl
ip addr show ens160
ip route
nmcli device status
getent hosts peer-node.internal
```

Expected documented example:

```text
Hostname:       almalinux-node02.lfcs.lab
Primary IPv4:   192.168.10.12/24
Secondary IPv4: 192.168.10.22/24
Gateway:        192.168.10.1
Peer:           192.168.10.11
IPv6:           disabled
```

---

# Result

The networking tasks were completed successfully across the Ubuntu and AlmaLinux lab systems.

The exercise provided practical experience with:

* Hostname management
* Netplan
* NetworkManager
* Static IPv4 configuration
* Secondary IPv4 addresses
* DNS configuration
* IPv6 configuration
* `/etc/hosts`
* Network troubleshooting and recovery

The lab also reinforced the importance of understanding administrative commands before executing them.
