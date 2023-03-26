---
title: Rootme  -  TryHackMe Writeup
author: D_captain
date: 2022-12-10 00:00:00 +0300
categories: [tryhackme, write up]
tags: [tryhackme, write up, walkthrough, ctf, thm, rootme, php, bypass, upload, privilege escalation, netacat, apache, nmap, gobuster, file upload, shell, suid, tryhackme walkthrough, tryhackme writeup, d_captain]
math: true
mermaid: true
image:
  path: https://cdn-images-1.medium.com/max/2368/1*-OBtC8ObjmYVcdyXpLDTGg.png
---

A ctf for beginners, can you root me?

[Room link here](https://tryhackme.com/room/rrootme)

## **Task 1 — Deploy the machine**

![](https://cdn-images-1.medium.com/max/2390/1*aYA-3lhFkpYlg-1AYVDoNg.png)

## **Task 2 — Reconnaissance**

### 1. Scan the machine, how many ports are open?

Let’s do a simple nmap scan.

    sudo nmap -sV 10.10.67.173

![](https://cdn-images-1.medium.com/max/2000/1*Jv7x5s2bBIl25HfasWOLeg.png)

### 2. What version of Apache is running?
### 3. What service is running on port 22?
**See above

### 4. Find directories on the web server using the GoBuster tool.**

    gobuster dir -u http://10.10.67.173 -w /usr/share/wordlists/dirb/common.txt

### 5. What is the hidden directory?**

![](https://cdn-images-1.medium.com/max/2000/1*mRZZy-SjzdhIUWR_ES0HKg.png)

## **Task 3 — Getting a shell**

**Find a form to upload and get a reverse shell, and find the flag.
user.txt**

Found a form in one of the hidden directories.

![](https://cdn-images-1.medium.com/max/2000/1*C9LJlfkxW5z4O9Vt5Mjc6g.png)

Let’s upload a php reverse shell by pentestmonkey.
[reverse shell here](https://github.com/pentestmonkey/php-reverse-shell).  
[shell walk through here](https://pentestmonkey.net/tools/web-shells/php-reverse-shell).

![](https://cdn-images-1.medium.com/max/2000/1*bePOSahkXTYQeLFgPYCW8g.png)

That is very not looking good, looks like our script upload was blocked.  
Let’s change the php file extension to “php5" and upload it again.  
(Explore on [file upload bypass](https://steflan-security.com/file-upload-restriction-bypass-cheat-sheet/) as this is about [File Upload Vilnerability](https://portswigger.net/web-security/file-upload))

![](https://cdn-images-1.medium.com/max/2000/1*V3gwad8g7ovBggHltHD3Gw.png)

We go on green.
Listen to the shell with netcat(changed my listening port).

    nc -lnvp 5555

Navigate to the php script and check netcat.  
(Damn! I always forget disabling my firewall — parrot)

Finally!

![](https://cdn-images-1.medium.com/max/2146/1*G8p-nIRREyS3lUDLqqgr_A.png)

Let’s search for the flag user.txt

    find . -name user.txt 2>/dev/null

![](https://cdn-images-1.medium.com/max/2000/1*RKhzF3YvUTjD6pg_8ykd2w.png)

Nice, kill that rat!

## **Task 4 Privilege escalation**

**(Usually my best parts)**

### 1. Search for files with SUID permission, which file is weird?.**

Using:

    find / -user root -perm /4000

Found python with SUID permissions meaning it does not drop elevated privileges.

![](https://cdn-images-1.medium.com/max/2000/1*gw25QJ7hivGvdZ2_69XHjg.png)

### 2. Find a form to escalate your privileges.**

Let’s get root access with:

    python -c 'import os; os.execl("/bin/sh", "sh", "-p")'

![](https://cdn-images-1.medium.com/max/2000/1*8Hl-UBfrLgxb_DleKF5bFA.png)

See [GTFOBins](https://gtfobins.github.io/gtfobins/python/#suid)

There we go, we are powerful now.

### 3. root.txt**

Let’s look for flag with:

    find . -name root.txt 2>/dev/null

![](https://cdn-images-1.medium.com/max/2000/1*GVKWpOvv41TG8SVzhTgJDg.png)

Nice, kill that rat!

Well, There are many ways of killing a rat!

[Originally posted on medium](https://d-captain.medium.com/rootme-tryhackme-df5dad43160d).

Happy Hacking.
