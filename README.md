# Detecting and removing malware using VirusTotal integration with Wazuh

In this project, I will be Integrating Virustotal with Wazuh to help detect and remove malware on the client machine (Windows 10)  after installing the wazuh (check my Wazuh-Deployment-with-File-Integrity-Monitoring-FIM-Implementation project) I followed the instructions on the Wazuh documentation page.

Wazuh's File Integrity Monitoring (FIM) module, combined with the VirusTotal API, provides an effective solution for detecting and mitigating file-based threats. The FIM module monitors directories for file changes and sends file hashes to VirusTotal for analysis. Using an API key for authentication, VirusTotal scans files and flags any malicious content. When a malicious file is detected, Wazuh triggers an active response script to remove the threat automatically.

# Configuration 
First, I had to enable the Virustotal module on the Wazuh manager cause by default it is  disabled.

<img width="1674" alt="Screenshot 2024-12-19 at 07 52 21" src="https://github.com/user-attachments/assets/17af8662-0fe1-45e2-8c6d-7925c791d436" />


Next, I had to make some changes to the "ossec.conf" file on Windows 10 (where the wazuh agent is installed). I enabled " syscheck" and added a new line of code to ensure that the downloads directory was monitored (This configuration allows the Wazuh agent to track changes in the specified directory effectively). After that, I saved the config and restarted the wazuh service

<img width="1022" alt="Screenshot 2024-12-19 at 15 40 40" src="https://github.com/user-attachments/assets/4be8fcde-c805-4475-b75a-bebb94dbb18d" />


<br>
<br>

Next, I downloaded and installed the Python executable installer from the Python official website.
<img width="862" alt="Screenshot 2024-12-19 at 16 17 26" src="https://github.com/user-attachments/assets/8ceffda4-52d8-4d50-a4f3-5f886ad3387b" />


Next, I created an active response script "remove-threat.py" to remove any malicious  files from the Windows endpoint. when I tried turning the script to an executable the script I encountered an error on line 108, it was an indentation error (it wasn't aligned correctly). After fixing that, the code ran properly. Then I moved the executable to the "C:\Program Files (x86)\ossec-agent\active-response\bin" directory. Next, I restarted the service and checked to ensure it was still running.

<img width="1020" alt="Screenshot 2024-12-19 at 17 00 07" src="https://github.com/user-attachments/assets/5e323a70-f439-4574-946c-52262aef1ca3" />

<img width="857" alt="Screenshot 2024-12-20 at 04 59 47" src="https://github.com/user-attachments/assets/87fc6f4e-3f25-48c3-be96-6484871392bf" />

<img width="1023" alt="Screenshot 2024-12-20 at 05 09 16" src="https://github.com/user-attachments/assets/bbe7f2b5-ce32-4ef2-8077-b52a46e2cc3d" />

<br>
<br>
<br>
<br>
To enable VirusTotal integration, I edited the /var/ossec/etc/ossec.conf file on the Wazuh server. I added the necessary configuration and replaced <YOUR_VIRUS_TOTAL_API_KEY> with my VirusTotal API key. This setup ensures that a VirusTotal query is triggered automatically whenever any rules in the FIM "syscheck" group are activated, enhancing the detection capabilities of the system.



<img width="1680" alt="Screenshot 2024-12-21 at 09 58 21" src="https://github.com/user-attachments/assets/ef4202ef-1533-4a54-9222-35ac9b8dc3a3" />

<br>
<br>
<br>
<br>
I updated the "local_rules.xml" file on the Wazuh server by adding rules to alert on Active Response results. This ensures I get notified whenever the system takes automated actions.
<img width="1680" alt="Screenshot 2024-12-21 at 10 26 07" src="https://github.com/user-attachments/assets/ddad7ee5-7d35-4339-bb82-8c91cd813798" />
