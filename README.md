# DNSExfiltration-base32-
## First Stage Download
The macro creates the PowerShell script, **dns.ps1**, the scheduled task, 
**taskscheduler.ps1**, and the VBScript, **update.vbs** (Figure 1). The VBScript will be 
launched every five minutes by the scheduled task. This VBScript performs the 
following operations:
1. Uses PowerShell to download a BAT file, **BAT.bat**, from the 
URL https://raw.githubusercontent.com/Valerie-Yeh/DNSExfiltration-base32-/main/BAT.bat and saves it in the directory
%PUBLIC%\AppData\Local\Temp\Update.
2. Executes the BAT file and stores the results in a file, out.txt, in the 
path %PUBLIC%\AppData\Local\Temp\Update.
3. Finally, it executes the PowerShell script, **dns.ps1**, which is used for the purpose of 
data exfiltration using DNS.
