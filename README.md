# DNSExfiltration-base32-
We aim to establish a real-time mechanism for detection on DNS-based data exfiltration. Our contribution is to develop a real-time DNS-based data exfiltration detection model, and we reinforce the capability of anomaly detection of our trained machine learning model with three different types of encoding methods, they are base32, base64 and hexadecimal encode after compression respectively.
## Attack and Data Collection
The organization, **APT34**, used Excel file with malicious macro embedded to download a BAT file and exfiltrated the result of the BAT script to their C2 server. The following operations are our reproduction of the attack.
### First Stage Download
The macro creates the PowerShell script, **dns.ps1**, the scheduled task, **taskscheduler.ps1**, and the VBScript, **update.vbs**. The VBScript will be launched every five minutes by the scheduled task. This VBScript performs the following operations:
1. Uses PowerShell to download a BAT file, **BAT.bat**, from the URL https://raw.githubusercontent.com/Valerie-Yeh/DNSExfiltration-base32-/main/BAT.bat and saves it in the directory %PUBLIC%\AppData\Local\Temp\Update. (This BAT file is used to collect important information from the system, including the currently logged on user, the hostname, network configuration data, user and group accounts, local and domain administrator accounts, running processes, and other data.)
2. Executes the BAT file and stores the results in a file, out.txt, in the path %PUBLIC%\AppData\Local\Temp\Update.
3. Finally, it executes the PowerShell script, **dns.ps1**, which is used for the purpose of data exfiltration using DNS.
### Data Exfiltration over DNS
The PowerShell script **dns.ps1** is used for this purpose. This script performs the following operations:
1. The text file containing the results of the BAT script will be encoded and uploaded to the C2 server, **%Your own C2 server%**, by embedding file data into part of the subdomain.
2. The script then quits, to be invoked again upon running the next scheduled task.
## Data Source and Attributes Extraction
A. Dataset
At the phase of data exfiltration over DNS, we encode the text file containing the results of the BAT script in three different ways, they are base64 encode, base32 encode, and hexadecimal encode after compression respectively. We then upload the three datasets to the C2 server and observe the logs on ELK, **Your own Kibana%**, simultaneously.
B. Query Name Attributes Engineering
The main achievement in this section is to identify that the eight attributes have strong predictive power in determining whether the query name is normal or malicious. The attributes include: (1) total count of characters in query name, (2) count of characters in subdomain, (3) count of uppercase characters, (4) count of numerical characters, (5) entropy, (6) number of labels, (7) maximum label length, and (8) average label length.
## Evaluation
We employ two machine learning techniques to determine if a DNS query is normal or not. One is Local Outlier Factor algorithm, and the other is Isolation Forest algorithm. Through our experiment, Isolation Forest outperforms Local Outlier Factor, and hence we adopt Isolation Forest algorithm to analyze the three different datasets.
By training a model with only normal query names, we aim to detect unknown malicious attacks. The machine is triggered by the eight attributes of each DNS query explained in the previous section. After the model is built, we apply Confusion Matrix to evaluate the accuracy.
