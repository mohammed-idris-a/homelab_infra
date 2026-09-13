# 04 — Packet Filtering & Port Redirection

Hands-on firewall and packet-filtering exercises completed as part of LFCS preparation.

This lab is organized by Linux distribution because the firewall management tools differ between RHEL-based and Debian/Ubuntu-based systems.

## Labs

| Lab                                                           | Distribution | Technology               | Status |
| ------------------------------------------------------------- | ------------ | ------------------------ | ------ |
| [04.1 — RHEL / AlmaLinux](./04.1-rhel-almalinux-firewalld.md) | RHEL-based   | firewalld / firewall-cmd | ✅      |
| 04.2 — Ubuntu                                                 | Ubuntu       | To be completed          | ⏳      |

## Topics

### 04.1 — RHEL / AlmaLinux

* Packet filtering
* TCP port management
* Port forwarding / redirection
* Masquerading / NAT
* IPv4 packet forwarding
* Rich rules
* Firewall verification
* Firewall troubleshooting

### 04.2 — Ubuntu

Ubuntu firewall configuration will be documented separately using the appropriate Ubuntu firewall tooling.

---

## Mental Model

```text
                    Network Traffic
                          │
          ┌───────────────┼────────────────┐
          │               │                │
          ▼               ▼                ▼
      Filtering       Forwarding         NAT
          │               │                │
          ▼               ▼                ▼
     Allow/deny       Redirect        Masquerade
       traffic         traffic         addresses
```

The purpose of this lab is to understand what each operation does and select the appropriate tool or rule for the scenario.
