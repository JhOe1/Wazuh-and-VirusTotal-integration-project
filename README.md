# Detecting and removing malware using VirusTotal integration with Wazuh

In this project, I will be Integrating Virustotal with Wazuh to help detect and remove malware on the client machine (Windows 10)  after installing the wazuh (check my Wazuh-Deployment-with-File-Integrity-Monitoring-FIM-Implementation project) I followed the instructions on the Wazuh documentation page.

Wazuh's File Integrity Monitoring (FIM) module, combined with the VirusTotal API, provides an effective solution for detecting and mitigating file-based threats. The FIM module monitors directories for file changes and sends file hashes to VirusTotal for analysis. Using an API key for authentication, VirusTotal scans files and flags any malicious content. When a malicious file is detected, Wazuh triggers an active response script to remove the threat automatically.

# Configuration 
First, I had to enable the Virustotal module on the Wazuh manager cause by default it is  disabled.

<img width="1674" alt="Screenshot 2024-12-19 at 07 52 21" src="https://github.com/user-attachments/assets/17af8662-0fe1-45e2-8c6d-7925c791d436" />


Next I had to make some changes to the "ossec.conf" file on Windows 10 (where the wazuh agent is installed). I ensure that syscheck was enabled and added a new line of code to ensure that the downloads directory was been monitored (This configuration allows the Wazuh agent to track changes in the specified directory effectively)

<img width="1022" alt="Screenshot 2024-12-19 at 15 40 40" src="https://github.com/user-attachments/assets/4be8fcde-c805-4475-b75a-bebb94dbb18d" />

