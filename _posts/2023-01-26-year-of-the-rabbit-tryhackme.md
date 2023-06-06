---
title: Year Of The Rabbit - Tryhackme Writeup
author: D_C4ptain
date: 2023-01-26 16:33:00 +0300
categories: [tryhackme, write up]
tags: [tryhackme, write up, walkthrough, ctf, thm, nmap, gobuster, burpsuite, hydra, ftp, cryptography, brute force, sudo, privilege escalation, shell, tryhackme walkthrough, tryhackme writeup, d_captain]
math: true
mermaid: true
image:
  path: https://cdn-images-1.medium.com/max/2000/1*BXp30fi4okA86U-T_DoToA.png
---

Time to enter the warren…  
[Room link](https://tryhackme.com/room/yearoftherabbit)

## Task 1

What is the user flag?

### Recon

Scanning the machine, we see that FTP, SSH and HTTP are running.

    nmap -sSCV 10.10.19.5 -v

![](https://cdn-images-1.medium.com/max/2000/1*Y8zcOG-t_g_4PRElcl9TSg.png)

Checking for a website, we are greeted with a default apache welcome page.

![](https://cdn-images-1.medium.com/max/2000/1*Xa_8mVtORuSVkA39qtOsQQ.png)

Checking for any directories with gobuster, I found a css file with some handy info.

    gobuster dir -u http://10.10.19.5 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50

![](https://cdn-images-1.medium.com/max/2000/1*v_A-7iQadP4sePjRzmoV2g.png)

![](https://cdn-images-1.medium.com/max/2000/1*WQP9KQouuWvpYLcNBAn1HA.png)

Accessing the secret page, prompts us to turn off javascript and we are redirected to “Rick Astley’s Never Gonna Give You Up”.

![](https://cdn-images-1.medium.com/max/2000/1*Uqp8_4FAsIFWM75GuC8XsA.png)

I’ve come across such a redirection before so I decided to check it out with burp.
I found an intermediary directory.
You can also easily see it on the browser developer tools, under network.

![with burpsuite.](https://cdn-images-1.medium.com/max/2000/1*zUpniXhuMwdEQk6aBqWxNw.png)*with burpsuite.*

![Developer tools, Network.](https://cdn-images-1.medium.com/max/2000/1*UC54cYSZ7prc55JX-iINAg.png)*Developer tools, Network.*

Navigating to the hidden directory, we get a png file.
I downloaded the file and as usual with images run ‘strings’ on it.
I found an ftp username with a bunch of random passwords.

![](https://cdn-images-1.medium.com/max/2000/1*Uk3AC1FD5az1uISFt-opfw.png)

### Brute force

Let’s dump these passwords to a file and brute force the ftp service with hydra.

    strings Hot_Babe.png >> ftppasses.txt
    
    (remove the unnecessary junk strings)
    
    hydra -l ftpuser -P ftppasses.txt ftp://10.10.19.5

![](https://cdn-images-1.medium.com/max/2000/1*kHaN5iS_Kq3sE8oAvTPPCA.png)

We can now log in to FTP.
Looking around we find some creds, let’s get them.

![](https://cdn-images-1.medium.com/max/2000/1*IIqn3tZ9SdgaC9LZZVAADA.png)

### Some cryptography

Opening the creds file reveals weird stuff.  
Ever heard of [brainfuck](https://en.wikipedia.org/wiki/Brainfuck)? I’ll leave you to that.

![](https://cdn-images-1.medium.com/max/2000/1*VYeAEjCKD88O61VGmq_9pA.png)

Let’s decode it [here](https://www.dcode.fr/brainfuck-language).

![](https://cdn-images-1.medium.com/max/2000/1*U1PS1sRJbzXZPh__9OLpvA.png)

Nice, Remember that SSH service? Let’s try logging in with these decrypted creds.

![](https://cdn-images-1.medium.com/max/2238/1*wVzvS1i1nIBXx3GodESQ4Q.png)

### Privilege escalation

Walking around, I found another user `Gwendoline` and our flag but we can’t read it.  
We need to become Gwendoline.

![](https://cdn-images-1.medium.com/max/2000/1*IVy3KFTZgc4nyKJMYx5H9Q.png)

From the note by root, we know there’s some secret place, let’s find it.

    locate s3cr3t

![](https://cdn-images-1.medium.com/max/2000/1*S_zYSWWrDwhsJQkZCPei7w.png)

Checking Gwendoline’s message, we get more creds.

![](https://cdn-images-1.medium.com/max/2000/1*JbF8Dc5ExA-Gcbk-hkinQw.png)

Let’s become Gwen and read our flag!

![](https://cdn-images-1.medium.com/max/2000/1*2s1YCyv0FyhmrfDzsh5UuQ.png)

## Task 2

What is the root flag?

Time to be root now! Before checking stuff with linpeas, let’s find low hanging fruits.

    sudo -l

![](https://cdn-images-1.medium.com/max/2152/1*RG8tIav75E02kurDLaOIKw.png)

Seems Gwen is not allowed to run vi as sudo.  
Checking the sudo version, I discovered it to be vulnerable to [CVE-2019–14287](https://www.exploit-db.com/exploits/47502)

![](https://cdn-images-1.medium.com/max/2000/1*H2TeUStcYPDgxH2Jep2UyQ.png)

We can run vi as sudo by providing the user id -1 which is translated to 0(root) by this sudo version.  
We will try to read the user.txt file.

    sudo -u#-1 /usr/bin/vi /home/gwendoline/user.txt

![](https://cdn-images-1.medium.com/max/2000/1*akYI_2LdzkSBwVJO_TJYHw.png)

Let’s escape vi by typing:

    :!/bin/bash

From the new root shell we can read our root flag.

![](https://cdn-images-1.medium.com/max/2000/1*7vZTg5QWuky6fSHLgeuDBQ.png)

If you liked it give it a thumbs up. Thanks to [MuirlandOracle](https://tryhackme.com/p/MuirlandOracle) for this box.

And by the way, There are many ways of killing a rat!

[Originally posted on medium](https://d-captain.medium.com/year-of-the-rabbit-tryhackme-writeup-87bd1a63bb49).

Happy Hacking.
