---
title: SheHacksKE KCA Intervarsity CTF — 2022 Write up
author: D_C4ptain
date: 2022-08-26 07:00:00 +0300
categories: [shehackske, write up]
tags: [shehackske, write up, walkthrough, ctf, osint, forensics, fr334aks, safaricom, africahackon, ekraal, trendmicro,microsoft, ransomware, gandcrab, base64, cyberchef, d_captain, D_C4ptain]
math: true
mermaid: true
image:
  path: https://cdn-images-1.medium.com/max/2000/1*S0c_Kg2yztHU_7QzdmxsMw.png
---

A short write up for some ctf challenges held at KCA University during the intervarsity bootcamp by [SheHacksKE](https://www.shehackske.com/), [fr334aks](https://fr334aks.github.io/), [Safaricom](https://www.safaricom.co.ke/), [AfricaHackOn](https://africahackon.com/), [Ekraal Innovation Hub](https://ekraal.org/), [Trend Micro](https://www.trendmicro.com/en_za/business.html), [Microsoft](https://www.microsoft.com/en-us/).

[Challenge link](https://kca.ciphercode.dev/)

## OSINT
### Challenge: Hunter1

Given a SHA256 hash of a malicious office document,
What’s the name of the ransomware behind the Malicious Office Document?

![](https://cdn-images-1.medium.com/max/2000/1*q8uUj_15V677nDChFy1mcA.png)

This was pretty easy.

### Solution

Searching the SHA256 hash on Google brings up a link highlighting “Gandcrab ransomware” and that’s it, you have hunted down the name of the ransomware.  
flag{gandcrab}

### Challenge: Last Hunt

This hunt was about the payment methods used by gandcrab.
Other than BTC the Ransomware group also uses?

![](https://cdn-images-1.medium.com/max/2000/1*ZJ1-g4-Pxs5B0PAbUAhwUw.png)

### Solution

I did some googling on “gandcrab ransomware” and went through a few articles.

![](https://cdn-images-1.medium.com/max/2000/1*Wu7edNSyipaTSKP5il3epA.png)

A story featured on malwarebytes [(here)](https://www.malwarebytes.com/gandcrab) stated the payment method.

![](https://cdn-images-1.medium.com/max/2040/1*zNyQ5DDRDlPv908jxuDlPA.png)

flag{dash}


## Forensics
### Challenge: Chatty Chatty — 2

“See ye and you shall find me”. Dig deeper to earn the second flag.  
I didn’t get the first flag but here, I could “see and find”  
I just saw and found.

![](https://cdn-images-1.medium.com/max/2000/1*7TLw6eHDumUxoovpMBztWw.png)

Download the given file.  
Well, turns out there are many ways of killing a rat(including burning the whole house)

### Solution

I ran strings on the file and found a base64 string.

![](https://cdn-images-1.medium.com/max/2000/1*GHYDRC3A-LNJ6G1Eq-95bg.png)

![](https://cdn-images-1.medium.com/max/2000/1*HnJD5_AZfF04r2MQQzhNag.png)

I decoded the base64 string to get the flag.

![](https://cdn-images-1.medium.com/max/2000/1*xyVYIdzOd3uGoityyVuFgQ.png)

Flag{I_Kn3w_Y0u_w0ulD_f1nd_me}

Similarly:
Uploading the file at [cyberchef](https://gchq.github.io/CyberChef/) also revealed the base64 without any recipe.

![Go ahead and decode the string.](https://cdn-images-1.medium.com/max/2712/1*Hh81xDWr9XZ0uRJfEms3cA.png)*Go ahead and decode the string.*

Similarly:
The exiftool should reveal the base64 string as explained by oste [here](https://05t3.github.io/posts/SheHacksInterUniCTF/).

Turns out; `There are many ways of killing a rat!`

[Originally posted on medium](https://d-captain.medium.com/shehackske-kca-intervarsity-ctf-2022-write-up-742226eb18f3)

Happy Hacking.
