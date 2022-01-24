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
// What users/localgroups are on the machine?
net users
net localgroups

// More info about a specific user. Check if user has privileges.
net user user1

// View Domain Groups
net group /domain

// View Members of Domain Group
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

## Privesc
