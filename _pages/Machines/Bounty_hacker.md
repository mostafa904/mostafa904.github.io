---
title: "Bounty Hacker"
tags:
    - machine
    - THM
date: "2024-05-28"
thumbnail: "/assets/img/thumbnail/bounty_hacker.jpeg"
bookmark: true
---

# Description
---

<img src="/assets/img/thumbnail/bounty_hacker.jpeg" alt="Bounty Hacker">

---

# Bounty Hacker Write-up

Hello guys, today we’ll talk about one of the TryHackMe rooms called “Bounty Hacker”. It’s a quite easy room in which you’ll learn about:

- FTP
- Hydra tool
- How you can do privilege escalation with the help of the tar command.

So, let’s get into it.

## Recon

I found 3 open ports by running:

```bash
nmap ip of machine  -T4
```
<img src="/assets/img/machines/bounty_hacker/nmap.jpg" alt="Bounty Hacker">
we see that port 21  and has FTP service, so let's try to connect to ftp with anonymous login
```bash
ftp ip
```
<img src="/assets/img/machines/bounty_hacker/ftp.jpg" alt="Bounty Hacker">
after connected -> run ls command and  found 2 files:
1-locks.txt
2-task.txt
we get them by `get` command then cat task.txt we find

## 2nd task

who wrote the task list
second task → LIN

and locks.txt has many of passwords :
<img src="/assets/img/machines/bounty_hacker/pass.jpg" alt="Bounty Hacker">

## 3rd task 
(What service can you bruteforce with the text file found?)

SSH

What is the users password?

so we ll try to crack the password of username we found (lin) using Hydra
```bash
hydra -l lin -P locks.txt ssh://machine ip 
```
<img src="/assets/img/machines/bounty_hacker/hydra.jpg" alt="Bounty Hacker">

the password :`RedDr4gonSynd1cat3`

## User's Flag

now we have a credential so let's login by ssh:
```bash
shh lin@ip of machine
password: RedDr4gonSynd1cat3
```
<img src="/assets/img/machines/bounty_hacker/user.jpg" alt="Bounty Hacker">

after logging in, run `ls` to see what u have
i found file called user.txt when we `cat` it , ll give me the content

user.txt → `THM{CR1M3_SyNd1C4T3}`

## Privilege Escalation

i tried many things but nothing worked to get this file.
i tried `ls -la` ,checking the history,listing processes but nothing worked. so i tried:
```bash
sudo -l
```
to List User’s Privileges:

<img src="/assets/img/machines/bounty_hacker/sudo.jpg" alt="Bounty Hacker">

Now we see that we have root access at `/bin/tar` command.
 After searching about how can i exploit this to read file from root,I found this:

<a href="https://gtfobins.github.io/gtfobins/tar/"</a>


<img src="/assets/img/machines/bounty_hacker/gtf.jpg" alt="Bounty Hacker">

after i ran these commands i got the flag:

<img src="/assets/img/machines/bounty_hacker/flag.jpg" alt="Bounty Hacker">

Root.txt → `THM{80UN7Y_h4cK3r}`

gg we are done! ty for readings, I hope it make sense.
