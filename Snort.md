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
Install SNORT from apt repository:

```plaintext
sudo apt install snort
```
Configure SNORT:

Edit the configuration files in /etc/snort directory as per your requirements.
```plaintext
sudo nano /etc/snort/snort.conf
```

Start SNORT:
```plaintext
sudo snort -A console -q -u snort -g snort -c /etc/snort/snort.conf -i <interface>
```

Replace `interface` with the interface you want to monitor. For example: eth0.
You can use `ifconfig` to find interface names.


## Configuration:

The configuration file for Snort is typically located at /etc/snort/snort.conf . Configuration is divided into several sections.

1) Set the network variables.

2) Configure the decoder

3) Configure the base detection engine

4) Configure dynamic loaded libraries

5) Configure preprocessors

6) Configure output plugins

7) Customize your rule set

8) Customize preprocessor and decoder rule set

9) Customize shared object rule set

### Data Acquisition (DAQ)

The DAQ module is responsible for capturing network packets and passing them to the Snort detection engine for analysis.

Let's see each line and its components:

config daq: <type>

This line specifies the type of DAQ module to use. The <type> parameter can be one of the following:

`pcap`: Uses the pcap library for packet capture on Unix-based systems.

`afpacket`: Uses the AF_PACKET interface for packet capture on Linux systems.

`dump`: Reads packets from a packet capture file (PCAP format).

`nfq`: Uses the Netfilter Queue (NFQUEUE) interface for packet capture.

`ipq`: Uses the old iptables QUEUE interface for packet capture.

`ipfw`: Uses the IPFW firewall interface for packet capture on BSD systems.

config daq_dir: <dir>

This line specifies the directory where Snort should look for the DAQ module shared objects (.so files). The <dir> parameter is the path to the directory containing the DAQ modules.

config daq_mode: <mode>

This line specifies the mode of operation for the DAQ module. The <mode> parameter can be one of the following:

`read-file`: Reads packets from a packet capture file.

`passive`: Captures packets passively without modifying or interfering with the network traffic.

`inline`: Operates in inline mode, where packets are actively intercepted and processed in real-time.

config daq_var: <var>

This line allows you to specify arbitrary variables to be passed to the DAQ module. The <var> parameter should be in the format <name>=<value>. These variables are used to configure specific options or behavior of the selected DAQ module.

