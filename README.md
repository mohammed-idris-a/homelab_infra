# Homelab Infrastructure 🏠🔧

A hands-on homelab environment for practicing Linux system administration, infrastructure concepts, troubleshooting, and preparation for the **Linux Foundation Certified System Administrator (LFCS)** certification.

This repository documents practical lab exercises, configurations, troubleshooting scenarios, and lessons learned while working with Linux systems.

## 🎯 Purpose

This homelab is used to:

* Practice Linux administration in an isolated environment
* Prepare for the LFCS certification through hands-on exercises
* Practice networking, storage, services, security, and troubleshooting
* Test configurations and administrative procedures
* Investigate and resolve system problems
* Document practical solutions and lessons learned

## 📚 Areas of Practice

The lab will progressively cover areas such as:

* Linux system administration
* Users and groups
* File ownership and permissions
* Processes and services
* `systemd`
* Package management
* Storage and filesystems
* LVM
* Networking
* SSH
* Firewall configuration
* Logs and system monitoring
* Task scheduling
* Shell scripting
* Troubleshooting

Additional infrastructure topics may be added as the lab evolves.

## 🖥️ Lab Environment

The repository is backed by a small virtualized Linux lab environment.

Details about the lab architecture and resources are documented separately:

**[Lab Environment](./docs/lab-environment.md)**

> Public documentation uses sanitized hostnames, IP addresses, and configuration values. Actual private lab details are intentionally not published.

## 🧪 Learning Approach

```text
Learn
  ↓
Build
  ↓
Test
  ↓
Break
  ↓
Troubleshoot
  ↓
Understand
  ↓
Document
```

The objective is not simply to memorize commands for an exam.

The focus is on understanding how Linux systems work and developing the ability to configure, diagnose, and troubleshoot them in a practical environment.

## 📂 Repository Structure

```text
homelab_infra/
│
├── docs/
│   └── lab-environment.md
│
├── linux/
│   ├── 01-networking-hostname-resolution/
│   │   └── README.md
│   │
│   ├── 02-time-synchronization/
│   │   └── README.md
│   │
│   └── 03-openssh-client-server/
│       └── README.md
│
└── README.md
```

New directories and labs will be added as actual exercises are completed.

## 📖 Labs

### Linux / LFCS

| Lab                                                                                        | Description                                                                                                    | Status |
| ------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------- | ------ |
| [01 — Networking, Hostname & Local Resolution](./linux/01-networking-hostname-resolution/) | Hostname configuration, Netplan, NetworkManager, static IPv4, DNS, IPv6, local resolution, and troubleshooting | ✅      |
| [02 — Time Synchronization](./linux/02-time-synchronization/) | Chrony, systemd-timesyncd, NTP configuration, synchronization verification, and troubleshooting | ✅ |
| [03 — OpenSSH Client & Server](./linux/03-openssh-client-server/) | SSH key authentication, server hardening, configuration validation, service reload, and authentication testing | ✅ |

More labs will be added as they are completed.

## 🚧 Status

This is an evolving hands-on learning repository.

The contents reflect practical lab work and troubleshooting performed during Linux and LFCS preparation rather than a predefined list of completed skills.

## 🔐 Security

This is a public repository.

The following must never be committed:

* Passwords
* API keys
* Access tokens
* SSH private keys
* Cloud credentials
* VPN credentials
* Sensitive configuration
* Public/home network details

Where exact environment values are not required, sanitized or example values are used.

## 👤 Author

**Mohammed Idris**

GitHub: [@mohammed-idris-a](https://github.com/mohammed-idris-a)

---

> **Build it. Break it. Troubleshoot it. Understand it. 🚀**
