# AnoxOS 🛡️

**AnoxOS** is an amnesic, privacy-hardened operating system built on Debian, designed for environments where security and data integrity are non-negotiable. It features a unique self-defense mechanism that ensures system binaries remain untouched by malicious actors.

## 🧠 Core Security Concept: Integrity Guard
The system utilizes a custom **Integrity Guard** that calculates a cryptographically keyed MAC (Message Authentication Code) for critical binaries (e.g., `/bin/bash`). 
* **Dynamic Keys:** A fresh secret key is generated on every boot, stored in volatile memory.
* **Tamper Proofing:** A kernel-level stealth daemon monitors the environment. If the hash of a protected binary changes, the guard flags the tampering.
* **Hardened Access:** The security daemon is protected by `chattr +i` (immutability) and hidden from process lists via a custom kernel module.

## ⚙️ How to Operate
AnoxOS uses a "burn-after-reading" approach for administrative access:
1. **Access Command:** Run `mykey` in your terminal.
2. **One-Time Token:** Select option `1` to display the session-specific security token for 5 seconds.
3. **Burned:** After 5 seconds, the token is permanently wiped from the disk and replaced with a `BURNED` flag.
4. **Administration:** Run `mykey` again, select option `2`, and input the token to enter the **Stealth Control Panel**.

### Administrative Panel
* **Activate/Deactivate:** Toggle the Integrity Guard. When deactivated, the `chattr` locks are removed to allow system maintenance.
* **Live Logs:** View real-time integrity verification logs via `journalctl`.

## 🛠 Deployment
1. Clone the repo: `git clone https://github.com/YOUR_REPO.git`
2. Run `./install.sh` to configure Debian 13 mirrors and security tools.
3. Your system is now fortified.

## 🌐 Tor-Shield Architecture
AnoxOS enforces total network anonymity using `iptables` and Tor Transparent Proxying.
* **Kernel-Level Filtering:** The system employs `iptables` NAT rules to force all TCP traffic and DNS requests through the Tor daemon. 
* **Leak Prevention:** Any application attempting to bypass the Tor proxy (non-Tor traffic) is automatically dropped by the kernel firewall. 
* **Global Proxy:** You don't need to configure proxy settings in your browser. Every application is "Tor-aware" by default.

### Network Management
You can control the Tor state at any time:
* `sudo anox-net tor` — Engage the Tor-Shield (default).
* `sudo anox-net clear` — Disable isolation (use only for maintenance/updates).
* `curl https://check.torproject.org/api/ip` — Verify your current anonymous status.
