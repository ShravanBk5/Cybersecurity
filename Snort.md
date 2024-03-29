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

### Network Variables

Setting the network variables in SNORT involves defining specific parameters that determine the range of network traffic monitored and analyzed by the Snort detection engine. These variables, configured in the SNORT configuration file (snort.conf), include:

`HOME_NET`: Represents the network or IP range considered the "internal" or trusted network. Snort monitors traffic originating from or destined to IP addresses within this range.

`EXTERNAL_NET`: Represents the network or IP range considered the "external" or untrusted network. Snort monitors traffic originating from or destined to IP addresses outside this range.

These network variables are essential for Snort to accurately identify and analyze network traffic, enabling it to distinguish between internal and external sources and destinations. By configuring these variables appropriately, Snort can effectively detect and respond to potential threats and intrusions within the monitored network environment.


### Data Acquisition (DAQ)

The DAQ module is responsible for capturing network packets and passing them to the Snort detection engine for analysis.

Let's see each line and its components:

`config daq: <type>`

This line specifies the type of DAQ module to use. The <type> parameter can be one of the following:

`pcap`: Uses the pcap library for packet capture on Unix-based systems. It supports passive mode, where Snort monitors network traffic without interfering with it. However, pcap does not support inline mode. 

`afpacket`: Uses the AF_PACKET interface for packet capture on Linux systems. afpacket supports both passive and inline modes. In passive mode, Snort captures network traffic without altering it, while in inline mode, Snort actively inspects and modifies network traffic in real-time.

`dump`: Reads packets from a packet capture file (PCAP format). It does not support passive or inline modes since it operates on pre-captured packet data from a file.

`nfq`: Uses the Netfilter Queue (NFQUEUE) interface for packet capture. 

`ipq`: Uses the old iptables QUEUE interface for packet capture.

`ipfw`: Uses the IPFW firewall interface for packet capture on BSD systems.



`Passive mode`: In this mode, Snort captures packets passively without interfering with the network traffic. It operates in a non-intrusive manner, observing the network traffic without actively blocking or modifying it. Passive mode is typically used for monitoring and analysis purposes, where the goal is to observe network activity without affecting its flow.

`Inline mode`: In contrast, inline mode involves actively intercepting and processing packets in real-time. Snort operates as an inline intrusion prevention system (IPS), where it can inspect, block, or modify network traffic as it passes through the network interface. Inline mode is used for actively enforcing security policies, such as blocking malicious traffic or preventing attacks in real-time.

`config daq_dir: <dir>`

This line specifies the directory where Snort should look for the DAQ module shared objects (.so files). The <dir> parameter is the path to the directory containing the DAQ modules.

`config daq_mode: <mode>`

This line specifies the mode of operation for the DAQ module. The <mode> parameter can be one of the following:

`read-file`: Reads packets from a packet capture file.

`passive`: Captures packets passively without modifying or interfering with the network traffic.

`inline`: Operates in inline mode, where packets are actively intercepted and processed in real-time.

`config daq_var: <var>`

This line allows you to specify arbitrary variables to be passed to the DAQ module. The <var> parameter should be in the format <name>=<value>. These variables are used to configure specific options or behavior of the selected DAQ module.


### Rulesets

Customize your rule set

This section includes a local.rules file located in the $RULE_PATH directory. These are custom rules created by the user to address specific needs or threats in their environment.

site specific rules

include $RULE_PATH/local.rules

The include directives specify additional rule files to be included in the rule set. 

include $RULE_PATH/app-detect.rules

include $RULE_PATH/attack-responses.rules

include $RULE_PATH/backdoor.rules

include $RULE_PATH/bad-traffic.rules

include $RULE_PATH/blacklist.rules

include $RULE_PATH/botnet-cnc.rules

include $RULE_PATH/browser-chrome.rules

include $RULE_PATH/browser-firefox.rules

...

Example of a rule to generate alert on possible SQL injection attempt.
```plaintext
alert tcp any any -> any any (msg:"Possible SQL injection attempt detected"; \
    flow:to_server,established; content:"SELECT"; nocase; \
    pcre:"/\b(SELECT|UPDATE|DELETE|INSERT)\b/i"; \
    sid:1000001;)
```

Explanation:

`alert tcp any any -> any any`: This specifies that the rule applies to TCP traffic from any source IP address and port to any destination IP address and port.

`(msg:"Possible SQL injection attempt detected";`: This provides a message to be logged when the rule triggers, indicating a possible SQL injection attempt.

`flow:to_server,established;`: This specifies that the rule should only be applied to traffic flowing to the server side of the connection, and the connection must be established.

`content:"SELECT"`; nocase;: This checks for the case-insensitive occurrence of the string "SELECT" in the packet payload.

`pcre:"/\b(SELECT|UPDATE|DELETE|INSERT)\b/i";`: This uses a Perl Compatible Regular Expression (PCRE) to search for SQL keywords (SELECT, UPDATE, DELETE, INSERT) with word boundary \b and case-insensitive /i matching.

`sid:1000001;`: This assigns a unique identifier (SID) to the rule, which can be used for reference and management.

Rule application order: pass->drop->sdrop->reject->alert->log


`Pass`: Snort checks if the packet matches any pass rules first. If a pass rule is matched, Snort allows the packet to continue without any further processing.

`Drop`: If no pass rules are matched, Snort checks if the packet matches any drop rules. If a drop rule is matched, Snort drops the packet and does not process it any further.

`Sdrop (Stream Drop)`: Stream drop rules are similar to drop rules but are specifically used for stream processing. If a sdrop rule is matched, Snort drops the packet but continues processing subsequent packets in the same stream.

`Reject`: If no pass or drop rules are matched, Snort checks if the packet matches any reject rules. If a reject rule is matched, Snort sends a TCP reset (RST) packet to the sender, effectively rejecting the packet.

`Alert`: If no pass, drop, sdrop, or reject rules are matched, Snort checks if the packet matches any alert rules. If an alert rule is matched, Snort generates an alert and logs information about the packet.

`Log`: Finally, if no pass, drop, sdrop, reject, or alert rules are matched, Snort checks if the packet matches any log rules. If a log rule is matched, Snort logs information about the packet but does not generate an alert.
