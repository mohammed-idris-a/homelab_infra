# Lab Environment 🖥️

## Overview

This homelab provides an isolated environment for Linux system administration practice and LFCS preparation.

The environment uses multiple Linux virtual machines so that networking, services, administration, and troubleshooting can be practiced in a controlled environment.

## Virtual Machines

The public documentation uses sanitized names and example network values.

| Node               | Operating System | Example Role                                     |
| ------------------ | ---------------- | ------------------------------------------------ |
| `ubuntu-node01`    | Ubuntu           | Linux administration and networking practice     |
| `almalinux-node02` | AlmaLinux        | Linux administration and NetworkManager practice |
| `lab-node03`       | Linux            | Additional lab and infrastructure experiments    |

## Example Network

The following values are examples used for documentation:

| Setting                | Value              |
| ---------------------- | ------------------ |
| Network                | `192.168.10.0/24`  |
| Gateway                | `192.168.10.1`     |
| Ubuntu primary IP      | `192.168.10.11/24` |
| Ubuntu secondary IP    | `192.168.10.21/24` |
| AlmaLinux primary IP   | `192.168.10.12/24` |
| AlmaLinux secondary IP | `192.168.10.22/24` |

These addresses are **documentation examples and do not represent the actual private lab network**.

## Virtualization

The Linux systems run as virtual machines to provide an isolated environment for experimentation and troubleshooting.

Some workloads may require virtualization features to be exposed to the guest operating system.

## Lab Goals

The environment is used to practice:

* Linux system administration
* Networking
* Storage
* Users and permissions
* Services and processes
* Security
* Troubleshooting
* Shell scripting
* LFCS-related administration tasks

## Documentation

Lab-specific configurations, commands, troubleshooting incidents, and lessons learned are documented in the relevant directories.

## Security

Actual private infrastructure details are intentionally excluded from this public repository.

Documentation should not contain:

* Real credentials
* Private keys
* Tokens
* Public IP addresses
* Home network information
* Sensitive host configuration

Sanitized values should be used whenever exact environment details are not required.
# Lab Environment 🖥️

## Overview

This document describes the virtualized lab environment used for Linux administration practice and LFCS preparation.

The environment is designed to provide an isolated setup where Linux configuration, networking, storage, services, and troubleshooting can be practiced safely.

## Virtual Machines

The lab consists of multiple Linux virtual machines.

| VM    | Operating System          | Purpose                                     |
| ----- | ------------------------- | ------------------------------------------- |
| VM 01 | Linux [Ubuntu]            | LFCS / administration                       |
| VM 02 | Linux [RHEL]              | Linux administration / cross-distro testing |
| VM 03 | Linux [Docker/Kubernetes] | Containers / Pods lab                       |

Detailed specifications will be documented as the environment evolves.

## Virtualization

The Linux systems run as virtual machines to provide an isolated environment for experimentation and troubleshooting.

## Lab Goals

The environment is used to practice:

* Linux system administration
* Networking
* Storage
* Users and permissions
* Services and processes
* Security
* Troubleshooting
* Shell scripting
* LFCS-related administration tasks

## Documentation

Configuration changes and important troubleshooting findings will be documented alongside the relevant labs.

## Security

The lab should not contain real production credentials or sensitive information.

Any credentials or sensitive configuration shown in documentation should use placeholders.
