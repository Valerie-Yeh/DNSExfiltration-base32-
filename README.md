# DNSExfiltration-base32-
We aim to establish a real-time mechanism for detection on DNS-based data exfiltration. Our contribution is to develop a real-time DNS-based data exfiltration detection model, and we reinforce the capability of anomaly detection of our trained machine learning model with three different types of encoding methods, they are base32, base64 and hex, respectively.
## Attack and Data Collection
### First Stage Download
The macro creates the PowerShell script, **dns.ps1**, the scheduled task, **taskscheduler.ps1**, and the VBScript, **update.vbs**. The VBScript will be launched every five minutes by the scheduled task. This VBScript performs the following operations:
1. Uses PowerShell to download a BAT file, **BAT.bat**, from the URL https://raw.githubusercontent.com/Valerie-Yeh/DNSExfiltration-base32-/main/BAT.bat and saves it in the directory %PUBLIC%\AppData\Local\Temp\Update. (This BAT file is used to collect important information from the system, including the currently logged on user, the hostname, network configuration data, user and group accounts, local and domain administrator accounts, running processes, and other data.)
2. Executes the BAT file and stores the results in a file, out.txt, in the path %PUBLIC%\AppData\Local\Temp\Update.
3. Finally, it executes the PowerShell script, **dns.ps1**, which is used for the purpose of data exfiltration using DNS.
### Data Exfiltration over DNS
The PowerShell script **dns.ps1** is used for this purpose. This script performs the following operations:
1. The text file containing the results of the BAT script will be encoded and uploaded to the C2 server, %Your own C2 server%, by embedding file data into part of the subdomain.
2. The script then quits, to be invoked again upon running the next scheduled task.
## Data Source and Attributes Extraction
