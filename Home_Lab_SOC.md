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
# Create NAT Network in VirtualBox
# File → Tools → Network Manager → NAT Networks
# Name: LabNet
# CIDR: 10.0.0.0/24
# Enable DHCP: Yes
```

### 2. Wazuh Server Setup
```bash
# Install Wazuh all-in-one
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
sudo bash wazuh-install.sh -a -o

# Pin Wazuh packages to prevent auto upgrade
sudo apt-mark hold wazuh-manager
sudo apt-mark hold wazuh-indexer
sudo apt-mark hold wazuh-dashboard

# Access dashboard
# https://10.0.0.4
```

### 3. Windows AD Setup
```powershell
# Install AD DS Role via Server Manager
# Add Roles and Features → Active Directory Domain Services

# Promote to Domain Controller
# New forest → lab.local

# Create test users
New-ADUser -Name "Test User" `
  -SamAccountName "testuser" `
  -UserPrincipalName "testuser@lab.local" `
  -AccountPassword (ConvertTo-SecureString "Password123!" -AsPlainText -Force) `
  -Enabled $true `
  -PasswordNeverExpires $true

New-ADUser -Name "Test User2" `
  -SamAccountName "testuser2" `
  -UserPrincipalName "testuser2@lab.local" `
  -AccountPassword (ConvertTo-SecureString "Password123!" -AsPlainText -Force) `
  -Enabled $true `
  -PasswordNeverExpires $true

New-ADUser -Name "Test User3" `
  -SamAccountName "testuser3" `
  -UserPrincipalName "testuser3@lab.local" `
  -AccountPassword (ConvertTo-SecureString "Password123!" -AsPlainText -Force) `
  -Enabled $true `
  -PasswordNeverExpires $true

# Create LinuxAdmins group
New-ADGroup -Name "LinuxAdmins" `
  -GroupScope Global `
  -GroupCategory Security

# Add Administrator to LinuxAdmins
Add-ADGroupMember -Identity "LinuxAdmins" -Members "Administrator"
```

### 4. Ubuntu Client Domain Join
```bash
# Set DNS to WinAD
sudo nano /etc/resolv.conf
# Add: nameserver 10.0.0.20

# Make DNS permanent
sudo nano /etc/systemd/resolved.conf
# DNS=10.0.0.20
# FallbackDNS=8.8.8.8
# Domains=lab.local
sudo systemctl restart systemd-resolved

# Install required packages
sudo apt update
sudo apt install realmd sssd sssd-tools adcli krb5-user \
  samba-common samba-common-bin packagekit -y

# Configure Kerberos
sudo nano /etc/krb5.conf
# default_realm = LAB.LOCAL

# Discover domain
realm discover lab.local

# Join domain
sudo realm join lab.local -U Administrator@LAB.LOCAL

# Allow domain users
sudo realm permit --all

# Enable home directory auto creation
sudo pam-auth-update --enable mkhomedir

# Give LinuxAdmins group sudo access
sudo nano /etc/sudoers.d/domain-admins
# Add: %LinuxAdmins@lab.local ALL=(ALL) ALL
```

### 5. Wazuh Agent Installation (Linux)
```bash
# Add Wazuh repository
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | \
  sudo gpg --dearmor -o /usr/share/keyrings/wazuh.gpg

echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] \
  https://packages.wazuh.com/4.x/apt/ stable main" | \
  sudo tee /etc/apt/sources.list.d/wazuh.list

sudo apt update

# Install agent
sudo WAZUH_MANAGER="10.0.0.4" apt install wazuh-agent -y

# Start agent
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent

# Pin agent version
sudo apt-mark hold wazuh-agent
```

### 6. Wazuh Agent Installation (Windows)
```powershell
# Download and install agent
msiexec.exe /i wazuh-agent-4.14.4-1.msi /q `
  WAZUH_MANAGER="10.0.0.4" `
  WAZUH_AGENT_NAME="WinAD"

# Start agent service
NET START WazuhSvc
```

---

## 📊 Wazuh Monitoring

### What is Monitored

| VM | Monitored Events |
|---|---|
| UbuntuServer | System logs, auth logs, file integrity |
| WinAD | AD logins, user changes, group changes, policy changes |
| UbunClient | SSH logins, domain auth, sudo usage, file integrity |

### Alert Levels

| Level | Severity | Example |
|---|---|---|
| 1-3 | Low | Informational events |
| 4-6 | Medium | Failed login attempts |
| 7-10 | High | Multiple failures, suspicious activity |
| 11-15 | Critical | Active attacks, rootkits |

### MITRE ATT&CK Coverage

| Technique | ID | Detected By |
|---|---|---|
| Password Guessing | T1110.001 | SSH brute force alerts |
| Valid Accounts | T1078 | Domain login monitoring |
| Lateral Movement via SSH | T1021.004 | SSH login alerts |

---

## 🔒 Security Testing

### Test 1 — SSH Brute Force
```bash
# Generate failed SSH logins
ssh wronguser@localhost
# Check Wazuh dashboard for T1110.001 alert
```

### Test 2 — Domain Login Monitoring
```bash
# Login as testuser
su - testuser@lab.local
# Check Wazuh for domain authentication event
```

### Test 3 — Privilege Escalation Attempt
```bash
# Login as testuser and try sudo
su - testuser@lab.local
sudo apt update
# Wazuh should alert on unauthorized sudo attempt
```

---

## 📸 Screenshots

> Add screenshots here after completing setup

- [ ] VirtualBox VM list
- [ ] NAT Network configuration
- [ ] Wazuh Dashboard overview
- [ ] Active agents in Wazuh
- [ ] Sample security alert
- [ ] MITRE ATT&CK mapping
- [ ] AD Users and Computers
- [ ] UbunClient domain join
- [ ] testuser login

---

## 📝 Lessons Learned

- Always pin Wazuh packages after install to prevent unintended upgrades
- Use NAT Network (not regular NAT) for VM-to-VM communication
- Use fully qualified username `Administrator@LAB.LOCAL` for domain joins
- Always take VirtualBox snapshots before major changes
- Ubuntu Server is better than Desktop for Wazuh agent (less RAM)
- ext4 partitions support online resizing with growpart + resize2fs
