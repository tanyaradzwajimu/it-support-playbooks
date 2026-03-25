# Remote Linux (Ubuntu) access and system update via TeamViewer

## Overview

| Field | Detail |
|---|---|
| **Category** | Remote support / Linux administration |
| **OS** | Ubuntu (Linux) — remote machine |
| **Tool used** | TeamViewer (free/personal licence) |
| **Access method** | Windows host → TeamViewer → Ubuntu terminal |
| **Skill level** | Intermediate |

---

## Scenario

A remote Ubuntu machine required a system update. Physical access was not available. A TeamViewer session was established from a Windows host to the Ubuntu machine, and the terminal was used to verify network connectivity and run system updates.

---

## What was observed

On connecting via TeamViewer, the terminal was opened and `ip a` was run to verify network status before proceeding with updates.

**Network interface output:**

```
2: enp2s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 — state DOWN
   link/ether b8:88:e3:d3:73:42

3: wlp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 — state UP
   inet 192.168.1.173/24 brd 192.168.1.255 scope global dynamic
   inet6 2605:59c1:... (multiple IPv6 addresses assigned)
```

**Key findings:**
- `enp2s0` (Ethernet/wired NIC) — **DOWN**, no cable connected (NO-CARRIER)
- `wlp3s0` (WiFi) — **UP and active**, assigned IP `192.168.1.173/24`
- Machine has internet connectivity via WiFi — safe to proceed with updates

![TeamViewer session showing ip a output in Ubuntu terminal](assets/TeamViewer_Accessing_Linux_Ubuntu_Distribution.png)

---

## Pre-update checklist

Before running updates on a remote machine, always verify:

- [ ] Network interface is UP and has a valid IP address
- [ ] Machine has sufficient disk space (`df -h`)
- [ ] No other users are active on the machine
- [ ] You have sudo privileges
- [ ] TeamViewer session is stable — do not run updates over an unstable connection

---

## Step-by-step procedure

### Step 1 — Establish TeamViewer session

1. Open TeamViewer on the Windows host
2. Enter the remote machine's TeamViewer ID and password
3. Confirm the session is active and the remote desktop is visible
4. Open a terminal on the Ubuntu machine (`Ctrl + Alt + T` or right-click desktop → Open Terminal)

---

### Step 2 — Verify network connectivity

Always check the network before running updates on a remote machine. If the connection drops mid-update, it can leave packages in a broken state.

```bash
# Check all network interfaces and their status
ip a

# Confirm internet connectivity
ping -c 4 google.com

# Optional: check which interface is being used for routing
ip route show
```

**Expected output for a healthy connection:**
- At least one interface showing `state UP` with a valid `inet` (IPv4) address
- Ping responses with 0% packet loss

---

### Step 3 — Check available disk space

```bash
df -h
```

Ensure the root partition (`/`) has at least **500MB free** before updating. Updates can fail or corrupt if disk space runs out mid-process.

---

### Step 4 — Run system update

```bash
# Update the package list
sudo apt update

# Review what will be upgraded (optional but good practice)
apt list --upgradable

# Upgrade all packages
sudo apt upgrade -y

# Full upgrade including dependency changes (use with caution)
sudo apt full-upgrade -y

# Remove packages no longer needed
sudo apt autoremove -y

# Clean up cached package files
sudo apt clean
```

---

### Step 5 — Check if a reboot is required

```bash
# Check if a reboot is needed after updates
cat /var/run/reboot-required
```

If the file exists, the system needs a reboot. **Before rebooting a remote machine:**

> ⚠️ **Warning:** Rebooting a remote machine will terminate your TeamViewer session. Only reboot if you are confident TeamViewer is configured to auto-start on boot, or if you have an alternative way back in.

```bash
# Verify TeamViewer service is set to start on boot
sudo systemctl is-enabled teamviewerd

# If not enabled, enable it before rebooting
sudo systemctl enable teamviewerd

# Then reboot
sudo reboot
```

After reboot, wait 2–3 minutes, then reconnect via TeamViewer.

---

### Step 6 — Verify update success

After updating (and rebooting if required):

```bash
# Check last update log
cat /var/log/apt/history.log | tail -50

# Confirm system is up to date
sudo apt update && apt list --upgradable
```

A clean system should return: `Listing... Done` with no packages listed as upgradable.

---

## Common issues and fixes

| Issue | Cause | Fix |
|---|---|---|
| `sudo apt update` fails with connection error | No internet / DNS issue | Run `ping 8.8.8.8` — if it pings but `ping google.com` fails, DNS is broken. Run `sudo systemctl restart systemd-resolved` |
| `enp2s0` is DOWN | No Ethernet cable plugged in | Use WiFi (`wlp3s0`) or plug in cable — not an error if WiFi is active |
| TeamViewer session drops during update | Unstable WiFi | Reconnect and check if update completed with `apt list --upgradable` |
| `dpkg` lock error | Another process is using apt | Wait 5 mins or run `sudo rm /var/lib/dpkg/lock-frontend` |
| Reboot required but TeamViewer won't reconnect | TeamViewer not set to auto-start | Enable via `sudo systemctl enable teamviewerd` before next reboot |
| Disk full during update | Insufficient space | Free space with `sudo apt autoremove && sudo apt clean` |

---

## Notes and best practices

- Always run `ip a` **before** starting updates on a remote machine — confirm you have a stable active interface
- The `enp2s0` interface being DOWN is **not an issue** as long as `wlp3s0` (WiFi) is UP and has an IP
- On production or client machines, prefer scheduling updates during off-hours
- TeamViewer free licence is for **personal use only** — use a business licence for client/commercial environments
- Consider setting up **unattended-upgrades** on remote Linux machines for automatic security patches:
  ```bash
  sudo apt install unattended-upgrades
  sudo dpkg-reconfigure --priority=low unattended-upgrades
  ```

---

## Related playbooks

- `rdp-connection-issues.md`
- `anydesk-setup-guide.md`
- `dns-resolution-failures.md`

---

*Documented by: Tanya Jimu | Date: January 2026 | Category: Remote Support / Linux*
