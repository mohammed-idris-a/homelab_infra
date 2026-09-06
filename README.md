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
│   └── 01-networking-hostname-resolution/
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
# Homelab Infrastructure 🏠🔧

A hands-on homelab environment used for practicing Linux system administration, infrastructure concepts, troubleshooting, and preparing for the **Linux Foundation Certified System Administrator (LFCS)** certification.

This repository documents the infrastructure, configurations, experiments, and practical exercises performed in the lab.

## 🎯 Purpose

The homelab is used to:

* Practice Linux administration in a realistic environment
* Prepare for the LFCS certification through hands-on work
* Experiment with system configuration and administration
* Practice networking, storage, services, security, and troubleshooting
* Test commands and procedures before applying them in real environments
* Document solutions and lessons learned

## 🖥️ Lab Environment

The lab consists of virtual machines running Linux and is used to simulate a small infrastructure environment.

Detailed VM configuration and network information are documented in:

**[Lab Environment](./docs/lab-environment.md)**

## 📚 Areas of Practice

The lab will progressively cover areas such as:

* Linux installation and configuration
* Users and groups
* File permissions and ownership
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
* System administration

Additional infrastructure technologies may be introduced as the lab evolves.

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

The goal is to understand how Linux systems work and develop the ability to configure, diagnose, and troubleshoot them in a practical environment.

## 📂 Repository Structure

```text
homelab_infra/
├── docs/
│   └── lab-environment.md
└── README.md
```

Directories will be added as actual labs and exercises are completed.

## 🚧 Status

This is an actively evolving homelab used for continuous hands-on learning and LFCS preparation.

The repository documents actual lab work rather than a predefined list of technologies.

## 🔐 Security

No passwords, private keys, API keys, tokens, cloud credentials, or other sensitive information should be committed to this repository.

Use placeholders for sensitive values when documenting configurations.

## 👤 Author

**Mohammed Idris**

GitHub: [@mohammed-idris-a](https://github.com/mohammed-idris-a)

---

> **Build it. Break it. Troubleshoot it. Understand it. 🚀**
