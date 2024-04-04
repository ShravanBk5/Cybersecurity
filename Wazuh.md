# Wazuh

Wazuh is a free and open source platform used for threat prevention, detection, and response. It is capable of protecting workloads across on-premises, virtualized, containerized, and cloud-based environments.

Wazuh solution consists of an endpoint security agent, deployed to the monitored systems, and a management server, which collects and analyzes data gathered by the agents. Besides, Wazuh has been fully integrated with the Elastic Stack, providing a search engine and data visualization tool that allows users to navigate through their security alerts.

## Wazuh capabilities

A brief presentation of some of the more common use cases of the Wazuh solution.

**Intrusion detection**

Wazuh agents scan the monitored systems looking for malware, rootkits and suspicious anomalies. They can detect hidden files, cloaked processes or unregistered network listeners, as well as inconsistencies in system call responses.

**Log data analysis**

Wazuh agents read operating system and application logs, and securely forward them to a central manager for rule-based analysis and storage. When no agent is deployed, the server can also receive data via syslog from network devices or applications.

**File integrity monitoring**

Wazuh monitors the file system, identifying changes in content, permissions, ownership, and attributes of files that you need to keep an eye on. In addition, it natively identifies users and applications used to create or modify files.

**Vulnerability detection**

Wazuh agents pull software inventory data and send this information to the server, where it is correlated with continuously updated CVE (Common Vulnerabilities and Exposure) databases, in order to identify well-known vulnerable software.

**Configuration assessment**

Wazuh monitors system and application configuration settings to ensure they are compliant with your security policies, standards and/or hardening guides. Agents perform periodic scans to detect applications that are known to be vulnerable, unpatched, or insecurely configured.

**Incident response**

Wazuh provides out-of-the-box active responses to perform various countermeasures to address active threats, such as blocking access to a system from the threat source when certain criteria are met.

**Regulatory compliance**

Wazuh provides some of the necessary security controls to become compliant with industry standards and regulations. 

**Cloud security**

Wazuh helps monitoring cloud infrastructure at an API level, using integration modules that are able to pull security data from well known cloud providers, such as Amazon AWS, Azure or Google Cloud.

**Containers security**

Wazuh provides security visibility into your Docker hosts and containers, monitoring their behavior and detecting threats, vulnerabilities and anomalies. 
## WUI

The Wazuh WUI provides a powerful user interface for data visualization and analysis. This interface can also be used to manage Wazuh configuration and to monitor its status.

**Modules overview**

![app](https://github.com/ShravanBk5/Cybersecurity/assets/68052087/498e4437-8c6b-494d-b21a-9a11567deff1)

**Security events**

![app2](https://github.com/ShravanBk5/Cybersecurity/assets/68052087/c9e928ae-6fd2-4f10-b532-b13d34ffdcd3)

**Integrity monitoring**

![app3](https://github.com/ShravanBk5/Cybersecurity/assets/68052087/835631d9-c011-49d0-ac4b-314748b75cdf)

**Vulnerability detection**

![app4](https://github.com/ShravanBk5/Cybersecurity/assets/68052087/9bd4467a-7822-4eb8-a4f1-a3a42a34aca8)

**Regulatory compliance**

![app5](https://github.com/ShravanBk5/Cybersecurity/assets/68052087/2386b642-de4f-4c0d-8ab8-74082ec4e6e7)

**Agents overview**

![app6](https://github.com/ShravanBk5/Cybersecurity/assets/68052087/40e05406-e660-40df-a194-f078705a7bb0)

**Agent summary**

![app7](https://github.com/ShravanBk5/Cybersecurity/assets/68052087/fa7bfb73-83d0-45a2-9507-983a5f7e3384)




![Wazuh diagram](https://github.com/ShravanBk5/Cybersecurity/assets/68052087/f5e70cb0-272f-48d5-83c9-37b6a18ac1d3)
