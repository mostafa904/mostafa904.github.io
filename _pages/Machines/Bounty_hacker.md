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

## First Task: Find Open Ports on the Machine

I found 3 open ports by running:

```bash
nmap <ip of machine> -T4
<img src="/assets/img/machines/bounty_hacker/nmap.jpg" alt="Bounty Hacker">
