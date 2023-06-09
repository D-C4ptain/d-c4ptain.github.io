---
title: Agent T - Tryhackme Writeup
author: D_C4ptain
date: 2023-02-26 10:33:00 +0300
categories: [tryhackme, write up]
tags: [tryhackme, write up, walkthrough, ctf, nmap, thm, whatweb, php, useragent, rce, python, enumeration, tryhackme walkthrough, tryhackme writeup, d_captain, D_C4ptain]
math: true
mermaid: true
image:
  path: https://cdn-images-1.medium.com/max/2000/1*tQUMFA98_57vBs9OtPDtVQ.png
---

Something seems a little off with the server.

Agent T uncovered this website, which looks innocent enough, but something seems off about how the server responds…
[Room here](https://tryhackme.com/room/agentt)

## Enumeration

    nmap -sV 10.10.146.43 -Pn -v

![](https://cdn-images-1.medium.com/max/2000/1*liWcNvYdx9rFc1Zejlb0ew.png)

Checking the web server:

![](https://cdn-images-1.medium.com/max/2486/1*XkX4McWvD4Kyfa9YNi7B5A.png)

Found nothing of interest on the website.
Directory fuzzing wasn’t helpful.

Checking with whatweb for site libraries and dependency versions:

    whatweb http://10.10.146.43/ -v


![](https://cdn-images-1.medium.com/max/2536/1*Kn4mGtMQcpf8rE0pDMpXQA.png)

Seeing the php version, I checked out on [exploit db](https://www.exploit-db.com/exploits/49933) and found it’s susceptible to an RCE through the user agent.

![](https://cdn-images-1.medium.com/max/2000/1*u1asUhge4Oj5u_bKxPIVog.png)

![](https://cdn-images-1.medium.com/max/2000/1*p8CaspWpYcwMpz--HOGZ8g.png)

## Getting that shell

Running the exploit, we get a root shell.
No privilege escalation today!

![](https://cdn-images-1.medium.com/max/2000/1*GkE7wONUq_io2eXwJN1oMg.png)

Let’s find that flag.
Found nothing on root and home directories.

Using find:

    find / -type f -name *.txt 2>/dev/null

![](https://cdn-images-1.medium.com/max/2000/1*gn4U_-bpQmi0SM2Cx4K7fQ.png)

There we go, kill that ratty flag!

![](https://cdn-images-1.medium.com/max/2000/1*sWmupK0Q8ckTBFciw2BUsw.png)

Easy peasy!
Thanks to [Ben](https://tryhackme.com/p/ben), [John Hammond](https://tryhackme.com/p/JohnHammond), [cmnatic](https://tryhackme.com/p/cmnatic), [blacknote](https://tryhackme.com/p/blacknote) and [timtaylor](https://tryhackme.com/p/timtaylor)

And by the way, There are many ways of killing a rat!

[Originally posted on medium](https://d-captain.medium.com/agent-t-tryhackme-writeup-cdfd779ba71f).

Happy Hacking.
