---
title: Huntress CTF 2023 Write up
author: D_C4ptain
date: 2023-10-03 10:15:00 +0300
categories: [CTF, write up]
tags: [HuntressCTF, October, Cybersecurity awareness, John hammond, Chris Cochran, fr334aks-mini, fr334aks]
---

This October - cybersecurity awareness month I was hunting, hacking and winning with team [Fr334aks-Mini](https://twitter.com/fr334aksmini) on [Huntress CTF 2023](https://huntress.ctf.games/)

![](/assets/img/posts/ctf/huntressctf23/huntress.png)

We delved into the intricacies of `Malware Analysis, Digital Forensics and Incident Response(DFIR), Cyber Threat Intelligence(CTI), Threat Hunting and much more` ; all crafted by [Huntress](https://www.huntress.com/) Security Researcher `John Hammond` and Chief Evangelist `Chris Cochran`

Here's one of the interesting challenges we tackled.


## 1. **Malware**

### a. Zerion

![](/assets/img/posts/ctf/huntressctf23/zerion1.png)

We are given a file `zerion`. First thing I do with files is check the file type. "zerion" has a PHP script with some other strings.

![](/assets/img/posts/ctf/huntressctf23/zerion1b.png)

Checking it I found the script using `base64` encoding and decoding, as well as `string manipulation functions` like `strrev` and `str_rot13`. The script uses `file_get_contents` to read its own source code, then splits the code into some array.
one of the elements of the array is decoded using `base64_decode`, then the string is `reversed` and `rotated` using strrev and str_rot13. 

![](/assets/img/posts/ctf/huntressctf23/zerion1c.png)

Just for the record, that md5-ish string didn't work.

![](/assets/img/posts/ctf/huntressctf23/zerion2.png)


I decided to do it's reverse with cyberchef. Decoded from ROT13 reversed the string and decoded from base64.

![](/assets/img/posts/ctf/huntressctf23/zerion3.png)

A mini-shell it was!








---

There are many ways of killing a rat!

Happy Hacking.