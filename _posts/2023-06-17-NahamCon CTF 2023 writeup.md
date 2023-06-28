---
title: NahamCon CTF 2023 Write up
author: D_C4ptain
date: 2023-06-17 11:33:00 +0800
categories: [CTF, write up]
tags: [NahamCon, write up, walkthrough, ctf, ghidra, forensics, pdf, pdf-parser, pwn, binary exploitation, d_captain, John Hammond, D_C4ptain, python]
math: true
mermaid: true
image:
  path: /assets/img/posts/ctf/nahamcon23/nahamcon23.png
---

![](/assets/img/posts/ctf/nahamcon23/teamcert.png)

My [team](https://twitter.com/fr334aksmini) and I participated in NahamCon CTF 2023 organized by [John Hammond](https://twitter.com/_johnhammond/). This is a write up of two of the challenges I solved.


## 1. Forensics

### a. Perfectly Disinfected

We are given a pdf document.

![](/assets/img/posts/ctf/nahamcon23/perfectlydis.png)

I ran the pdf on `pdf-parser` and got some interesting `null terminated` text.

![](/assets/img/posts/ctf/nahamcon23/perfectlydis1.png)

I wrote a short python code to remove the `escape sequence` characters.

![](/assets/img/posts/ctf/nahamcon23/perfectlydis2.png)

And there we go.

![](/assets/img/posts/ctf/nahamcon23/perfectlydis3.png)


---
## 2. PWN

### a. Binary Exploitation

We have a binary to pwn and it's code in C

![](/assets/img/posts/ctf/nahamcon23/sesame.png)

Accessing the server for the challenge promped for a cave opener!  
Decided to try out 1's

![](/assets/img/posts/ctf/nahamcon23/sesame1.png)

I tried more 1's and got an "incorrect password" message

![](/assets/img/posts/ctf/nahamcon23/sesame2.png)

Decided to open the binary with `Ghidra'. I looked around and found the function that was performing password checks.
This should be what I'm looking for!

![](/assets/img/posts/ctf/nahamcon23/sesame3.png)

This time I tried the password multiple times and the cave opened! Got our flag right there.

![](/assets/img/posts/ctf/nahamcon23/sesame4.png)

---

The CTF was really awesome, I learnt alot from other challenges too.

Oh! and again, there are many ways of killing a rat!

Happy Hacking.