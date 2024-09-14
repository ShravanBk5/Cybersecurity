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


## Network Diagram for my setup

![Wazuh diagram](https://github.com/ShravanBk5/Cybersecurity/assets/68052087/f5e70cb0-272f-48d5-83c9-37b6a18ac1d3)

## Installation

**VM** : 2 instances. One with Ubuntu Server and other with Ubuntu.

Installing Wazuh
----------------

Download and run the Wazuh installation assistant. Ex: ver 4.7

    curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a

Once the assistant finishes the installation, the output shows the access credentials and a message that confirms that the installation was successful.

        INFO: --- Summary ---
        INFO: You can access the web interface https://<wazuh-dashboard-ip>
            User: admin
            Password: <ADMIN_PASSWORD>
        INFO: Installation finished.
  
You now have installed and configured Wazuh.

Access the Wazuh web interface with ``https://<wazuh-dashboard-ip>`` and your credentials:

  Username: admin
  Password: <ADMIN_PASSWORD>


## Deploying Wazuh agents on Linux endpoints

The agent runs on the host you want to monitor and communicates with the Wazuh server, sending data in near real-time through an encrypted and authenticated channel.

The deployment of a Wazuh agent on a Linux system uses deployment variables that facilitate the task of installing, registering, and configuring the agent.

Add the Wazuh repository to download the official packages. Then deploy a Wazuh agent

1. Install the GPG key:

        curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644 /usr/share/keyrings/wazuh.gpg
   
2. Add the repository:

        echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list

3. Update the package information:

        curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644 /usr/share/keyrings/wazuh.gpg

4. Deploy a Wazuh agent. To deploy the Wazuh agent on your endpoint, select your package manager and edit the WAZUH_MANAGER variable to contain your Wazuh manager IP address or hostname.

        WAZUH_MANAGER="<WAZUH MANAGER IP>" apt-get install wazuh-agent

5. Enable and start the Wazuh agent service.

        systemctl daemon-reload
        systemctl enable wazuh-agent
        systemctl start wazuh-agent

## Monitoring Agents

### File integrity monitoring

The File Integrity Monitoring(FIM) module runs periodic scans on specific paths and monitors specific directories for changes in real time. You can set which paths to monitor in the configuration of the Wazuh agents and manager.

FIM stores the files checksums and other attributes in a local FIM database. Upon a scan, the Wazuh agent reports any changes the FIM module finds in the monitored paths to the Wazuh server.

![fim-flow1](https://github.com/ShravanBk5/Cybersecurity/assets/68052087/2dcfc03e-26d2-4cb2-bb87-fbeded75cda5)

### Configure the FIM module

Any time the FIM module runs a scan, it triggers alerts if it finds modified files and depending on the changed file attributes. You can view these alerts in the Wazuh dashboard.

Following, you can see how to configure the FIM module to monitor a file and directory. Replace ``FILEPATH/OF/MONITORED/FILE`` and ``FILEPATH/OF/MONITORED/DIRECTORY`` with your own filepaths. 

### Add the following settings to the Wazuh agent configuration file, replacing the directories values with your own filepaths:
  
``/var/ossec/etc/ossec.conf``
   
         <syscheck>
            <directories>FILEPATH/OF/MONITORED/FILE</directories>
            <directories>FILEPATH/OF/MONITORED/DIRECTORY</directories>
         </syscheck>

### Restart the Wazuh agent with administrator privilege to apply any configuration change:
        
        systemctl restart wazuh-agent

### VirusTotal integration

Wazuh detects malicious files through an integration with VirusTotal, a powerful platform aggregating multiple antivirus products and an online scanning engine. Combining this tool with our FIM module provides an effective way of inspecting monitored files for malicious content.

VirusTotal is an online service that analyzes files and URLs to detect viruses, worms, trojans, and other malicious content using antivirus engines and website scanners.

Public API

This method uses a free API with many of VirusTotal's functionalities. However, it has some significant limitations, such as Request rate limitations and low priority access for requests done by this API to the VirusTotal engine.

Sign up to VirusTotal to get access to Public API key.

Below is an example of settings you must add to the ``/var/ossec/etc/ossec.conf`` file on the Wazuh server:

    <integration>
      <name>virustotal</name>
      <api_key>API_KEY</api_key> <!-- Replace with your VirusTotal API key -->
      <group>syscheck</group>
      <alert_format>json</alert_format>
    </integration>

Add the following to the <syscheck> section of the configuration file. You can configure these options in the Wazuh server and the Wazuh agent ``/var/ossec/etc/ossec.conf`` file.

     <syscheck>
       <directories check_all="yes" realtime="yes">FILEPATH/OF/MONITORED/DIRECTORY</directories>
     </syscheck>

You can try to download a malicious file, for example, ``https://secure.eicar.org/eicar.com`` while monitoring the download folder to see Virustotal integration in action.

Warning: Download the Eicar file here for testing purposes only. Recommended to be tested in a sandbox, not in a production environment.

The file should be automatically deleted and logged.

![screencapture-192-168-29-39-app-wazuh-2024-04-10-21_52_20](https://github.com/ShravanBk5/Cybersecurity/assets/68052087/20372adf-7fdb-4024-9ddf-e7c0852f3e9d)

Here,

![screencapture-192-168-29-39-app-wazuh-2024-04-10-21_52_20 2](https://github.com/ShravanBk5/Cybersecurity/assets/68052087/8ab387aa-c1aa-4757-99f1-e8aade5cea25)
