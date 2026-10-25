<div align="center">

# 🔐 Cybersecurity Lab Environment Setup

**An isolated VirtualBox + Kali Linux lab for penetration-testing and ethical-hacking practice**

<p align="center">
  <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Ver-Virtualbox%20v7.2.6-0070C0?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Kali%20Linux-v2025.4-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" />
  <img src="https://img.shields.io/badge/Kali%20Linux-404040?style=flat-square&labelColor=C00000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Focus-Ethical_Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Michael%20Kamau%20-C00000?style=flat-square" />
</p>

---

## 📌 Project Overview

This project documents how I built a **virtual cybersecurity and penetration-testing lab** using VirtualBox and Kali Linux.

The lab gives me a safe, repeatable environment to practice network scanning, reconnaissance, vulnerability assessment and other authorized security-testing activities. It runs on a private virtual network, so more machines (including intentionally vulnerable targets) can be added in future projects.

## 🎯 Objectives

- [x] Install and configure VirtualBox
- [x] Install / import Kali Linux as a virtual machine
- [x] Create a private **NAT Network** for the lab
- [x] Configure and verify network connectivity on Kali
- [x] Assign a consistent IP address to the Kali VM
- [x] Take a clean snapshot for recovery
- [x] Document the full setup process
- [ ] Add target machines *(future work)*

## 🛡️ Purpose of the Lab

The lab can be used for:

- Network reconnaissance and port scanning
- Vulnerability assessment
- Packet analysis
- Web application security testing
- Exploitation practice on machines I own
- Experimenting with security tools

> ⚠️ **Important:** This lab must only be used against systems that I own or have explicit written permission to test. Never use these tools against unauthorized systems.

---

## 🏗️ Lab Architecture

```mermaid
flowchart TB
    Internet(("🌍 Internet"))
    subgraph Host["💻 Host: [Your Host OS]"]
        subgraph VB["VirtualBox"]
            NAT["🔀 NAT Network<br/>[10.0.0.0/24]<br/>Gateway [10.0.0.1]"]
            K["🐉 Kali Linux<br/>[10.0.0.2]"]
            F["🎯 Future targets<br/>[10.0.0.3 – 10.0.0.99]"]
        end
    end
    K --- NAT
    F -.-> NAT
    NAT --> Internet
```

<!-- Add your own network diagram screenshot here -->
<!-- ![Lab architecture](screenshots/1-lab-architecture.png) -->

Additional VMs can be attached to the same NAT Network later.

## ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration |
|---|---|
| 🖥️ Host OS | Windows 11 |
| 🧠 Host RAM | 16 GB |
| ⚡ Processor | Intel Core i5 |
| 🧰 Hypervisor | VirtualBox 7.2.6 |
| 🐉 Security OS | Kali Linux 2025.4 |
| 🧠 Kali RAM | 8192 MB |
| 🌐 Virtual Network | NAT Network |
| 📡 Network Address | 10.0.0.0/24 |
| 🐧 Kali IP Address | 10.0.0.2/24 |
| 🚪 Default Gateway | 10.0.0.1 |
| 🌍 DNS Server | 8.8.8.8 |
| 🔮 Reserved for future VMs | [10.0.0.3 – 10.0.0.99] |

---

## 🪜 Lab Setup Procedure

### Step 1 — Install 7-Zip
Kali's pre-built VM images are often distributed as `.7z` archives, so I installed 7-Zip to extract them.

### Step 2 — Install VirtualBox
Installed VirtualBox [version] as the hypervisor.

### Step 3 — Create the NAT Network
In **VirtualBox → Tools → Network** I created a dedicated NAT Network:

```text
Network Name : [NatNetwork]
IPv4 Prefix  : [10.0.0.0/24]
DHCP         : [Enabled]
IPv6         : [Disabled]
```

![NAT network settings](screenshots/2-nat-network-settings.png)

> **Why a NAT Network?** VMs on the same NAT Network can talk to each other *and* reach the internet through address translation. That is exactly what a multi-machine attacker/target lab needs. A plain "NAT" adapter keeps each VM isolated from the others.

### Step 4 — Import Kali Linux
Downloaded the official Kali VM from [kali.org](https://www.kali.org/get-kali/) and imported it into VirtualBox.

```text
Adapter 1
  Attached to  : NAT Network
  Network      : [NatNetwork]
  Adapter Type : [Intel PRO/1000 MT Desktop]

RAM : [2048 MB]
```

![Kali Linux VM](screenshots/3-kali-linux.png)

*(Optional)* A shared folder was set up to move files between the host and the VM.

### Step 5 — Configure the Kali Network
Configured a fixed IPv4 address so the machine is easy to reference in future exercises:

```text
IP Address : [10.0.0.2]
Netmask    : [255.255.255.0]
Gateway    : [10.0.0.1]
DNS        : [8.8.8.8]
```

![Kali network settings](screenshots/4-kali-network-settings.png)

### Step 6 — Create a Clean Snapshot
Once configuration was complete I took a snapshot named:

```text
Clean Kali - Network Setup
```

If a future exercise breaks the VM, I can roll back to this known-good baseline.

---

## 🔎 Lab Verification

| ✅ Test | 🧾 Command | 🎯 Expected Result | Result |
|---|---|---|---|
| Check IP address | `ip a` | Correct Kali IP shown | ✅ |
| Test gateway | `ping -c 4 [10.0.0.1]` | Replies received | ✅ |
| Test internet | `ping -c 4 8.8.8.8` | Replies received | ✅ |
| Test DNS | `nslookup example.com` | Domain resolves | ✅ |
| Verify Nmap | `nmap --version` | Version displayed | ✅ |
| Verify snapshot | Restore snapshot, run `ip a` | Baseline restored | ✅ |

<!-- Add a screenshot of your terminal output here -->
<!-- ![Verification](screenshots/5-verification.png) -->

---

## 🐞 Problems Encountered & Solutions

> Keep only the problems **you actually ran into**. Real troubleshooting notes are the most valuable part of a lab write-up.

### Problem 1 — [Short title, e.g. VirtualBox VT-x / virtualization error]
**Symptom:** [What went wrong, and the exact error message if you have it]

**Cause:** [e.g. hardware virtualization was disabled in BIOS/UEFI]

**Solution:**
1. [Step one]
2. [Step two]
3. [Step three]

### Problem 2 — [Short title, e.g. No internet after setting a static IP]
**Symptom:** [...]

**Cause:** [...]

**Solution:**
```bash
# example: find your real connection name first, then modify it
nmcli connection show
```

---

## 💡 What I Learned

1. **NAT vs NAT Network** — [your own explanation]
2. **VM networking** — [how adapters connect VMs to networks]
3. **Static IP configuration** — [IP, netmask, gateway, DNS in Kali]
4. **Snapshots** — take a clean one *before* risky experiments
5. **Documentation** — screenshots, commands and troubleshooting notes make a project reproducible and professional

---

## 🔐 Security & Ethical Use

This lab is for **education and authorized testing only**. All activity takes place on virtual machines I own inside an isolated virtual network.

## 🔗 Tools & Resources

- **7-Zip:** <https://7-zip.org/download.html>
- **VirtualBox:** <https://www.virtualbox.org/wiki/Downloads>
- **Kali Linux:** <https://www.kali.org/get-kali/>

## 👤 Author

**Michael Kamau**

[LinkedIn](https://www.linkedin.com/in/dabon-mikey/) · [GitHub](https://github.com/404-Noir)

---

**Program:** [Network Walks] | **Week:** [01] | **Project:** Cybersecurity & Pentesting Lab Setup
