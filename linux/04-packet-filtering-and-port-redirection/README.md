# 04 — Packet Filtering & Port Redirection

Hands-on firewall, packet-filtering, forwarding, and NAT exercises completed as part of LFCS preparation.

This lab focuses on understanding **how Linux handles network traffic** and how firewall and NAT requirements are implemented using distribution-specific tools.

The same networking concepts are practiced on both RHEL-based and Debian/Ubuntu-based systems.

## Labs

| Lab                                                           | Distribution | Technology                 | Status |
| ------------------------------------------------------------- | ------------ | -------------------------- | ------ |
| [04.1 — RHEL / AlmaLinux](./04.1-rhel-almalinux-firewalld.md) | RHEL-based   | firewalld / firewall-cmd   | ✅      |
| [04.2 — Ubuntu](./04.2-ubuntu-firewall.md)                    | Ubuntu       | UFW / iptables / netfilter | ✅      |

## Topics

### 04.1 — RHEL / AlmaLinux

* Packet filtering with firewalld
* TCP port management
* Port forwarding / redirection
* Masquerading / NAT
* IPv4 packet forwarding
* Rich rules
* Firewall verification
* Firewall troubleshooting
* Runtime firewall configuration

### 04.2 — Ubuntu

* Packet filtering with UFW
* Source-restricted firewall rules
* Numbered UFW rules and rule removal
* iptables / netfilter fundamentals
* Local port redirection with `REDIRECT`
* Destination NAT with `DNAT`
* Source NAT with `MASQUERADE`
* IPv4 packet forwarding
* Persistent iptables rules
* `iptables-persistent` / `netfilter-persistent`
* iptables chain and packet-flow troubleshooting

---

## Mental Model

Linux firewall and NAT configuration becomes easier to reason about when the requirement is translated into a traffic-flow problem first.

```text
                         Network Traffic
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
          Filtering        Forwarding           NAT
              │                │                │
              ▼                ▼                ▼
          Allow / Deny     Route / Redirect   Translate
             traffic          traffic         addresses
              │                │                │
              │                │         ┌──────┴──────┐
              │                │         │             │
              │                │       DNAT      MASQUERADE
              │                │         │             │
              │                │    Destination      Source
              │                │    translation    translation
              │                │
              │                └── REDIRECT
              │                     to local port
              │
              └── Source / port / protocol rules
```

### Core distinction

```text
Is traffic being allowed or denied?
        │
        └── Filtering

Is traffic being sent through this machine?
        │
        └── Forwarding

Is the destination being changed?
        │
        └── DNAT / REDIRECT

Is the source address being translated?
        │
        └── SNAT / MASQUERADE
```

The exact commands and configuration methods depend on the Linux distribution.

---

## Distribution Comparison

| Requirement            | RHEL / AlmaLinux                  | Ubuntu                                         |
| ---------------------- | --------------------------------- | ---------------------------------------------- |
| Host firewall          | firewalld                         | UFW                                            |
| Firewall CLI           | `firewall-cmd`                    | `ufw`                                          |
| Advanced filtering     | Rich rules                        | UFW rules / iptables                           |
| NAT / port translation | firewalld configuration           | iptables / netfilter                           |
| Port forwarding        | firewalld                         | iptables                                       |
| Masquerading           | firewalld                         | iptables                                       |
| IPv4 forwarding        | sysctl                            | sysctl                                         |
| Persistence            | firewalld permanent configuration | `iptables-persistent` / `netfilter-persistent` |

This comparison demonstrates that the **networking concepts remain the same while the administration interface and implementation differ between distributions**.

---

## Key Learning

The main objective of this lab is not memorizing firewall commands.

The focus is on being able to:

1. Identify the traffic requirement.
2. Determine whether the requirement involves filtering, forwarding, or NAT.
3. Identify the appropriate packet-processing stage.
4. Select the appropriate Linux firewall tool.
5. Construct the rule.
6. Verify the resulting configuration.
7. Troubleshoot incorrect rules.
8. Remove temporary/test rules.
9. Persist the intended configuration where required.

### Practical troubleshooting mindset

```text
Requirement
    ↓
Traffic flow
    ↓
Packet-processing stage
    ↓
Firewall / NAT rule
    ↓
Verification
    ↓
Troubleshooting
    ↓
Persistence
```

The individual distribution implementations are documented in:

* [04.1 — RHEL / AlmaLinux firewalld](./04.1-rhel-almalinux-firewalld.md)
* [04.2 — Ubuntu firewall](./04.2-ubuntu-firewall.md)
