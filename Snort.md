# SNORT (IDS/IPS)

SNORT is a powerful open-source intrusion detection system (IDS) and intrusion prevention system (IPS) that provides real-time network traffic analysis and data packet logging. SNORT uses a rule-based language that combines anomaly, protocol, and signature inspection methods to detect potentially malicious activity. 

## What Are the Features of SNORT?
There are various features that make SNORT useful for network admins to monitor their systems and detect malicious activity. These include:

Real-time Traffic Monitor

Packet Logging

Analysis of Protocol

Content Matching

OS Fingerprinting

Can Be Installed in Any Network Environment

Rules Are Easy to Implement

## Installation and Configuration of SNORT on Ubuntu Server:

To install SNORT on an Ubuntu Server, you can follow these steps:

```plaintext
sudo apt update
```
Install SNORT from apt repository

```plaintext
sudo apt install snort
```
Configure SNORT:

Edit the configuration files in /etc/snort directory as per your requirements.

Start SNORT:
```plaintext
sudo snort -A console -q -u snort -g snort -c /etc/snort/snort.conf -i <interface>
```

Replace `interface` with the interface you want to monitor. For example: eth0.
You can use `ifconfig` to find interface names.
