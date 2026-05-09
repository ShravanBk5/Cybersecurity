# 🛡️ Home Lab SOC Environment

A fully functional Security Operations Center (SOC) home lab built using VirtualBox, Wazuh SIEM, and Windows Active Directory for security monitoring practice and learning.

---

## 📋 Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [System Requirements](#system-requirements)
- [Network Configuration](#network-configuration)
- [Virtual Machines](#virtual-machines)
- [Software Versions](#software-versions)
- [Port Forwarding](#port-forwarding)
- [Domain Configuration](#domain-configuration)
- [Setup Guide](#setup-guide)
- [Wazuh Monitoring](#wazuh-monitoring)
- [Security Testing](#security-testing)
- [Screenshots](#screenshots)

---

## 🔭 Overview

This home lab simulates a real enterprise SOC environment with:
- **Wazuh SIEM** for centralized security monitoring
- **Windows Active Directory** for identity and access management
- **Domain joined Linux client** simulating employee workstations
- **Multiple user accounts** for simulating real user activity
- **MITRE ATT&CK** mapped alerts for threat detection practice

---

## 🏗️ Architecture

// IMAGE

---

## 💻 System Requirements

| Component | Spec |
|---|---|
| CPU | Intel Core i5-10210U |
| RAM | 16GB |
| Host OS | Windows |
| Hypervisor | VirtualBox 7.2.0 |
| Host IP | 192.168.1.107 |

---

## 🌐 Network Configuration

| Setting | Value |
|---|---|
| Network Type | NAT Network |
| Network Name | LabNet |
| Subnet | 10.0.0.0/24 |
| Gateway | 10.0.0.1 |
| DNS Server | 10.0.0.20 (WinAD) |

---

## 🖥️ Virtual Machines

| VM | Hostname | OS | IP | RAM | Storage | Role |
|---|---|---|---|---|---|---|
| Wazuh Server | UbuntuServer | Ubuntu Server 22.04 LTS | 10.0.0.4 | 3GB | 25GB | Wazuh Manager + Indexer + Dashboard |
| Domain Controller | WinAD | Windows Server 2022 Standard | 10.0.0.20 | 2GB | 50GB | Active Directory DC |
| Linux Client | UbunClient | Ubuntu Server 22.04 LTS | 10.0.0.30 | 1.5GB | 20GB | Domain joined client + Wazuh Agent |
| Standby | Ubuntu | Ubuntu Desktop 20.04 LTS | 10.0.0.10 | 2GB | 40GB | Standby / Future use |

---

## 📦 Software Versions

| Software | Version |
|---|---|
| Wazuh | 4.14.4 |
| Ubuntu Server | 22.04.5 LTS |
| Ubuntu Desktop | 20.04 LTS |
| Windows Server | 2022 Standard Evaluation |
| VirtualBox | 7.2.0 |
| Linux Kernel | 5.15.0-173-generic |

---

## 🔌 Port Forwarding

| Service | Host Port | Guest IP | Guest Port |
|---|---|---|---|
| Wazuh Server SSH | 2204 | 10.0.0.4 | 22 |
| Ubuntu Desktop SSH | 2210 | 10.0.0.10 | 22 |
| Wazuh Dashboard | 8443 | 10.0.0.4 | 443 |
| WinAD RDP | 33389 | 10.0.0.20 | 3389 |

---

## 🏢 Domain Configuration

| Setting | Value |
|---|---|
| Domain Name | lab.local |
| Realm | LAB.LOCAL |
| Domain Controller | WinAD (10.0.0.20) |
| Domain Admin | Administrator@lab.local |

### Domain Users

| Username | Type | Sudo on Linux | Purpose |
|---|---|---|---|
| Administrator@lab.local | Domain Admin | ✅ (LinuxAdmins group) | IT Admin |
| testuser@lab.local | Regular User | ❌ | Simulated employee 1 |
| testuser2@lab.local | Regular User | ❌ | Simulated employee 2 |
| testuser3@lab.local | Regular User | ❌ | Simulated employee 3 |

### AD Groups

| Group | Members | Purpose |
|---|---|---|
| LinuxAdmins | Administrator | Sudo access on Linux machines |
| Domain Admins | Administrator | Full AD control |

---

## 🔧 Setup Guide

### 1. VirtualBox NAT Network
```bash
