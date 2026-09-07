# Linux Time Synchronization — Chrony & systemd-timesyncd

## Overview

Hands-on time synchronization lab completed as part of LFCS preparation.

This lab covers:

* Inspecting NTP sources and synchronization status
* Configuring Chrony with an upstream NTP server
* Managing the Chrony service with `systemctl`
* Applying configuration changes immediately
* Forcing an immediate clock step with `chronyc makestep`
* Managing NTP synchronization with `systemd-timesyncd`
* Configuring a custom NTP server on Ubuntu
* Verifying synchronization using `chronyc` and `timedatectl`

> Hostnames, IP addresses, timestamps, and other environment-specific values shown in this document are sanitized for public documentation.

---

# Task 2.1 — Chrony Time Synchronization

**Documented Host:** `linux-node02`

## Objectives

* Inspect the current NTP sources and synchronization status
* Configure Chrony to use `time.google.com`
* Use the `iburst` option
* Ensure Chrony is running and enabled
* Apply configuration changes immediately
* Force an immediate clock correction when required

---

## 1. Inspect Current Time Sources

Check the current Chrony sources:

```bash
chronyc sources
```

Check detailed synchronization status:

```bash
chronyc tracking
```

Check the Chrony service:

```bash
systemctl status chronyd
```

---

## 2. Configure the NTP Server

Edit the Chrony configuration:

```bash
sudo vim /etc/chrony.conf
```

Configure the upstream time source using:

```text
server time.google.com iburst
```

> The exact configuration syntax depends on whether the `server` or `pool` directive is being used. The directive must be included; placing only `time.google.com` on a line results in an invalid Chrony configuration.

---

## 3. Validate and Restart Chrony

Restart the Chrony service:

```bash
sudo systemctl restart chronyd
```

Check the service:

```bash
sudo systemctl status chronyd
```

Ensure the service starts automatically at boot:

```bash
sudo systemctl enable chronyd
```

Verify:

```bash
systemctl is-enabled chronyd
systemctl is-active chronyd
```

---

## 4. Apply an Immediate Clock Step

To force Chrony to step the system clock immediately if an offset exists:

```bash
sudo chronyc makestep
```

Expected response:

```text
200 OK
```

---

## 5. Verify Synchronization

Check synchronization details:

```bash
chronyc tracking
```

Check available sources:

```bash
chronyc sources
```

The selected source should be indicated by `*` in the source list once synchronization has been established.

---

# Troubleshooting — Chrony Configuration Error

## Problem

After editing `/etc/chrony.conf`, restarting the service failed:

```text
Job for chronyd.service failed because the control process exited with error code.
See "systemctl status chronyd.service" and "journalctl -xeu chronyd.service" for details.
```

## Investigation

The service logs were checked as suggested by `systemctl`:

```bash
sudo journalctl -xeu chronyd.service
```

The relevant error indicated that `time.google.com` was being interpreted as an invalid directive:

```text
Fatal error : Invalid directive time.google.com
```

## Root Cause

The NTP hostname had been added without the required Chrony directive.

Chrony expects a directive such as:

```text
server time.google.com iburst
```

or, where appropriate:

```text
pool <pool-name> iburst
```

The hostname cannot be placed on its own.

## Resolution

The Chrony configuration was corrected to use the proper directive.

Chrony was then restarted successfully:

```bash
sudo systemctl restart chronyd
```

The service status was verified and synchronization was confirmed.

---

# Task 2.2 — Ubuntu Time Configuration

**Documented Host:** `linux-node01`

## Objectives

* Disable network time synchronization
* Verify that synchronization is inactive
* Re-enable network time synchronization
* Configure `systemd-timesyncd` to use `time.google.com`
* Verify the active synchronization source

---

## 1. Inspect systemd-timesyncd

Check the service:

```bash
sudo systemctl status systemd-timesyncd.service
```

Check the current time synchronization state:

```bash
timedatectl status
```

---

## 2. Toggle NTP Synchronization

For exam practice, NTP synchronization can be disabled using:

```bash
sudo timedatectl set-ntp false
```

Verify:

```bash
timedatectl status
```

Re-enable synchronization:

```bash
sudo timedatectl set-ntp true
```

Verify again:

```bash
timedatectl status
```

> `timedatectl set-ntp` controls network time synchronization. This is different from simply stopping or starting the `systemd-timesyncd` service.

---

## 3. Configure the NTP Server

Edit:

```bash
sudo vim /etc/systemd/timesyncd.conf
```

Configure:

```ini
[Time]
NTP=time.google.com
FallbackNTP=ntp.ubuntu.com
```

Restart the service:

```bash
sudo systemctl restart systemd-timesyncd.service
```

---

## 4. Verify the Configuration

Display the active timesync configuration:

```bash
timedatectl show-timesync
```

Verify the active synchronization status:

```bash
timedatectl timesync-status
```

The active server should show:

```text
Server: <resolved-address> (time.google.com)
```

The output can also be used to inspect:

* Stratum
* Poll interval
* Offset
* Delay
* Jitter
* Frequency

---

# Key Learnings

* Learned how to inspect NTP sources with:

  ```bash
  chronyc sources
  ```
* Learned how to view detailed Chrony synchronization performance with:

  ```bash
  chronyc tracking
  ```
* Learned how to configure an upstream Chrony server with the `iburst` option.
* Learned that a Chrony server or pool entry requires the appropriate directive; a hostname by itself is invalid syntax.
* Learned to use:

  ```bash
  journalctl -xeu chronyd.service
  ```

  to investigate why a systemd service failed to start.
* Learned how to ensure `chronyd` is both running and enabled at boot.
* Learned how to force an immediate clock step with:

  ```bash
  chronyc makestep
  ```
* Learned how to disable and re-enable network time synchronization with:

  ```bash
  timedatectl set-ntp false
  timedatectl set-ntp true
  ```
* Practiced configuring `systemd-timesyncd` with a custom NTP server.
* Learned how to use:

  ```bash
  timedatectl show-timesync
  ```

  and:

  ```bash
  timedatectl timesync-status
  ```

  to inspect the active synchronization state.
* Reinforced the importance of reading the systemd error message and following the suggested diagnostic command instead of guessing at the cause.

---

# Verification Summary

## Chrony

```bash
chronyc sources
chronyc tracking
systemctl is-enabled chronyd
systemctl is-active chronyd
```

## Ubuntu / systemd-timesyncd

```bash
timedatectl status
timedatectl show-timesync
timedatectl timesync-status
systemctl is-active systemd-timesyncd.service
```

---

# Result

The time synchronization exercises were completed using both **Chrony** and **systemd-timesyncd**.

The lab provided practical experience with NTP source configuration, service persistence, synchronization verification, immediate clock correction, and troubleshooting an invalid Chrony configuration.
