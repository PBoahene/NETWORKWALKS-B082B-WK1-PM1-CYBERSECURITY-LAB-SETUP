<div align="center">

# 🔐 Cybersecurity Lab Environment Setup

**Building an isolated virtual lab for penetration testing and ethical hacking practice**
</div>

<p align="center">
  <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Ver-Virtualbox%20v7.2-0070C0?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Kali%20Linux-v2026.2-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/OPNsense-Firewall%2FRouter-238F89?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" />
  <img src="https://img.shields.io/badge/Kali%20Linux-404040?style=flat-square&labelColor=C00000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Waqas%20Karim%20CCIE-C00000?style=flat-square" />
</p>

---

## 📌 Project Overview

This project focuses on setting up a **virtual cybersecurity and penetration-testing laboratory** using VirtualBox, Kali Linux, and **OPNsense** as a virtual firewall/router.

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly.

Rather than relying on VirtualBox's built-in NAT Network, this lab uses **OPNsense** to route and firewall an isolated **Internal Network**, more closely mirroring how a real enterprise network segments and controls traffic between machines.

---

## 🎯 Objectives

The main objectives of this project are to:

- Install and configure VirtualBox.
- Deploy **OPNsense** as a virtual firewall/router.
- Install/import Kali Linux as a virtual machine.
- Connect Kali Linux to an isolated **Internal Network** behind OPNsense.
- Configure OPNsense to handle routing, DHCP, and NAT for the internal segment.
- Configure network connectivity for Kali Linux and verify DNS resolution.
- Assign a consistent IP address to the Kali VM.
- Take a clean VM snapshot for recovery.
- Document the complete setup process.
- Prepare the environment for future cybersecurity projects (e.g. Metasploitable as a target).

---

## 🛡️ Purpose of the Lab

The lab provides an isolated and controlled environment for cybersecurity learning and authorized security testing.

It can be used for activities such as:

- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet analysis
- Web security testing
- Exploitation practice
- Firewall/router configuration practice
- Security-tool experimentation

⚠️ **Important:** This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.

---

## 🏗️ Lab Architecture

![](1-screenshot-title-image.png)

The lab consists of:
- **OPNsense** — acting as the virtual firewall/router, bridging the host's network to an isolated Internal Network ('LAN').
- **Kali Linux** — the attacker/testing machine, connected only to the Internal Network behind OPNsense.
- **Metasploitable 2** — a deliberately vulnerable target machine, available on the same lab for future exercises.

Additional target machines can be added to the same Internal Network in future projects.

---

## ⚙️ Lab Configuration

| 🧩 Component        | ⚙️ Configuration                          |
| -------------------- | ------------------------------------------ |
| 🖥️ Host OS          | Windows 10                                |
| 🧠 Host RAM          | 8 GB                                       |
| ⚡ Processor         | Intel Core i7                              |
| 🧰 Hypervisor        | VirtualBox 7.2                             |
| 🧱 Firewall/Router   | OPNsense (virtual appliance)               |
| 🐉 Security OS       | Kali Linux 2026.2                          |
| 🧠 Kali RAM          | 4096 MB                                    |
| 🧵 Kali Processors   | 4                                           |
| 🌐 Virtual Network   | Internal Network ('LAN', behind OPNsense)  |
| 🚪 Gateway           | OPNsense LAN interface                     |
| 🌍 DNS Server        | Provided/forwarded via OPNsense            |
| 💾 Kali Disk         | kali-linux-2026.2-virtualbox-amd64.vdi (80 GB) |
| 🎯 Target VM         | Metasploitable 2 (same lab)                |

*Note: Specific IP addressing depends on the OPNsense LAN interface configuration and is intentionally omitted from this public README for security reasons — see the "Security & Ethical Use" section below.*

---

# 🪜 Lab Setup Procedure

## Step 1. Install 7-Zip

7-Zip was installed to extract the Kali Linux virtual-machine package, which may be distributed as a `.7z` archive.

**Tool:** 7-Zip

---

## Step 2. Install VirtualBox

VirtualBox 7.2 was installed as the hypervisor.

---

## Step 3. Deploy OPNsense as a Virtual Firewall/Router

An OPNsense virtual machine was deployed in VirtualBox to act as the router and firewall for the lab.

Configuration:
```text
WAN Interface: Connected to host network (NAT/Bridged), for internet access
LAN Interface: Connected to an Internal Network ('LAN') in VirtualBox
DHCP:          Enabled on the LAN interface
```

An **Internal Network** was used (instead of VirtualBox's built-in NAT Network) so that OPNsense — not VirtualBox — is fully responsible for routing, NAT, and DHCP between the lab's internal machines. This is closer to how a real network segment sits behind a firewall.

---

## Step 4. Import Kali Linux

The Kali Linux virtual machine was downloaded from the official Kali Linux website and imported into VirtualBox.

The VM network adapter was configured as follows:

```text
Adapter 1
Attached to:  Internal Network
Network:      LAN
Adapter Type: Intel PRO/1000 MT Desktop
```

The VM was allocated:

```text
RAM:        4096 MB
Processors: 4
```
![](3-screenshot-kali-linux.png)

A shared folder was also configured for transferring required files between the host operating system and the Kali VM.

---

## Step 5. Configure the Kali Linux Network

Kali Linux was connected to the Internal Network behind OPNsense and received (or was assigned) an IP address from OPNsense's DHCP/LAN configuration.

Network connectivity, gateway reachability (OPNsense LAN IP), and DNS resolution were verified from within Kali.

![](4-screenshot-kali-network-settings.png)

---

## Step 6. Create a Clean VM Snapshot

After completing the initial configuration, a VirtualBox snapshot was created for the Kali VM.

Example snapshot name:

```text
Clean Kali - Network Setup
```

The snapshot represents the clean baseline of the laboratory.

If a future exercise changes or damages the VM configuration, the machine can be restored to this baseline.

---

# 🔎 Lab Verification

| ✅ Test                        | 🧾 Command                          | 🎯 Expected Result              |
| ----------------------------- | ------------------------------------ | -------------------------------- |
| 🌐 Check IP address           | `ip a`                                | Correct Kali IP displayed        |
| 📡 Test gateway               | `ping <OPNsense LAN IP>`             | Successful replies               |
| 🌍 Test Internet connectivity | `ping 8.8.8.8`                       | Successful replies               |
| 🔎 Test DNS resolution        | `nslookup networkwalks.com`          | Domain resolves                  |
| 🧰 Verify Nmap                | `nmap --version`                     | Nmap version displayed           |
| 🔄 Verify snapshot            | Restore snapshot and run `ip a`      | Baseline configuration restored  |

---

# 🐞 Problems Encountered & Solutions

Documenting problems is an important part of the project.

## Problem 1. Internet Connectivity After Static IP Configuration

After manually configuring the IPv4 settings, Internet connectivity may fail depending on the Kali/NetworkManager configuration.

One workaround used during this lab was:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

The network connection was then restarted/rebooted and connectivity was tested again.

> **Important:** Network interface and connection names may differ between systems. Students should first identify their actual connection name before running an `nmcli` command.

---

## Problem 2. Routing Through OPNsense (Internal Network vs NAT Network)

Since Internal Network mode in VirtualBox provides no DHCP or NAT by default (unlike NAT Network), Kali initially had no connectivity until OPNsense's LAN interface was properly configured to act as the DHCP server and gateway for the internal segment.

The issue was resolved by:

1. Confirming OPNsense's LAN interface was attached to the same Internal Network as Kali.
2. Enabling and verifying the DHCP server on OPNsense's LAN interface.
3. Confirming OPNsense's WAN interface had internet access (via NAT/Bridged mode to the host).
4. Restarting the Kali VM's network interface and re-testing connectivity (Kali → OPNsense LAN → OPNsense WAN → Internet).
5. Verifying DNS resolution was passing correctly through OPNsense.

---

# 💡 What I Learned

Through this project, I learned how to create and configure a virtual environment for cybersecurity practice, using a more realistic firewall-segmented architecture.

The most important concepts I learned include:

### 1. Internal Network vs NAT Network

An Internal Network is fully isolated and provides no routing, NAT, or DHCP on its own — everything must be handled by another VM (in this case, OPNsense) acting as a gateway. This differs from VirtualBox's built-in NAT Network, which handles routing and DHCP automatically. Using an Internal Network with a virtual firewall is more representative of real-world network segmentation.

### 2. Firewall/Router Configuration (OPNsense)

I learned how to deploy and configure a virtual firewall to manage traffic between an isolated internal segment and the outside network, including WAN/LAN interface roles, DHCP, and routing.

### 3. Virtual Machine Networking

I learned how VirtualBox virtual network adapters connect virtual machines to different types of networks and how network configuration affects communication between machines.

### 4. Static/DHCP IP Configuration

I learned how to configure and verify IPv4 addressing, subnet masks, gateways, and DNS settings in Kali Linux, and how these depend on the upstream router/firewall configuration.

### 5. VM Snapshots

I learned that a clean snapshot should be created **before performing risky or experimental activities**.

This provides a known-good recovery point for future cybersecurity exercises.

### 6. Documentation

I learned that documenting commands, configuration, screenshots, problems, and solutions is an important part of a professional cybersecurity project.

---

# 🔐 Security & Ethical Use

This laboratory is intended strictly for education purposes only. Specific internal IP addresses, hostnames, and OPNsense configuration details have been intentionally omitted or generalized in this public documentation.

---

# 🔗 Tools & Resources

- **7-Zip:** [https://7-zip.org/download.html](https://7-zip.org/download.html)
- **VirtualBox:** [https://virtualbox.org/wiki/Downloads](https://virtualbox.org/wiki/Downloads)
- **Kali Linux:** [https://kali.org/get-kali](https://kali.org/get-kali)
- **OPNsense:** [https://opnsense.org/download/](https://opnsense.org/download/)

---

# 👤 Author

**Boahene Prince**
Cybersecurity Intern, Batch B083B — Networkwalks

LinkedIn: [linkedin.com/in/boahene-prince-603b08372](https://www.linkedin.com/in/boahene-prince-603b08372/)

---

## 📌 Project Information

**Program Name:** Cybersecurity at Networkwalks | **Week:** 01 | **Project:** Cybersecurity & Pentesting Lab Setup (OPNsense + Kali Linux) | **Repository:** GitHub
