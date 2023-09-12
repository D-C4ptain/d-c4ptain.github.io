---
title: Patriot CTF 2023 Write up
author: D_C4ptain
date: 2023-09-11 18:33:00 +0800
categories: [CTF, write up]
tags: [PatriotCTF, George Mason University, GMU, MasonCC, write up, walkthrough, ctf, jeopardy, forensics, crypto, OSINT, misc, cyberchef, base64, exiftool, aircrack-ng]
math: true
mermaid: true
image:
  path: /assets/img/posts/ctf/patriotctf2023/patriot1.png
---

This weekend I Participated in [Patriot CTF](https://pctf.competitivecyber.club/) 2023 alongside my team [Fr334aks-Mini](https://twitter.com/fr334aksmini). 

![](/assets/img/posts/ctf/patriotctf2023/patriot2.png)
 
It was a jeopardy style ctf organised by the CyberSec club of George Mason University, MasonCC and these are some of the challenges I solved.

[Official writeups here](https://github.com/MasonCompetitiveCyber/PatriotCTF2023/)


## 1. **Forensics**

### a. Unsupported Format

![](/assets/img/posts/ctf/patriotctf2023/funsupportedformat1.png)

We are given a `Flag.jpeg` file which could not be opened. Checked it's metadata with exiftool and noticed something: GIMP and some embeded binary data.  

![](/assets/img/posts/ctf/patriotctf2023/funsupportedformat1b.png)

Opening the image with GIMP didn't really help.

![](/assets/img/posts/ctf/patriotctf2023/funsupportedformat2.png)
  
With cyberchef, I extracted a hidden image file. 

![](/assets/img/posts/ctf/patriotctf2023/funsupportedformat3.png)

![](/assets/img/posts/ctf/patriotctf2023/funsupportedformat4.png)

`PCTF{c0rrupt3d_b1t5_4r3_c00l}`


### b. Capybara

![](/assets/img/posts/ctf/patriotctf2023/fcapybara1.png)  

We are given an image of a capybara.

![](/assets/img/posts/ctf/patriotctf2023/capybara.jpeg)  

Checking the strings, there was several occurences of `audio.wav`

![](/assets/img/posts/ctf/patriotctf2023/fcapybara2.png)

![](/assets/img/posts/ctf/patriotctf2023/fcapybara3.png)

We go on to extract the audio file with binwalk.

![](/assets/img/posts/ctf/patriotctf2023/fcapybara4.png)

![](/assets/img/posts/ctf/patriotctf2023/fcapybara5.png)

Playing the audio showed it was on morse code. Let's decode it [here](https://morsecode.world/international/decoder/audio-decoder-adaptive.html)

![](/assets/img/posts/ctf/patriotctf2023/fcapybara6.png)

We get some hex data.

![](/assets/img/posts/ctf/patriotctf2023/fcapybara7.png)

Decoding it directly didn't work so I tickled the leading character-zero.

![](/assets/img/posts/ctf/patriotctf2023/fcapybara8.png)

`PCTF{d0_y0U_kN0W_h0W_t0_R34D_m0r53_C0d3?}`



## 2. **Crypto**

### a. Multi-numeral

![](/assets/img/posts/ctf/patriotctf2023/cmulti-numeral1.png)

We have a txt file with some `binary` data.

![](/assets/img/posts/ctf/patriotctf2023/cmulti-numeral2.png)

Decoding it gives data in `decimal` representation.

![](/assets/img/posts/ctf/patriotctf2023/cmulti-numeral3.png)

After decoding it we have `hex values`

![](/assets/img/posts/ctf/patriotctf2023/cmulti-numeral4.png)

Thi further leads to a `base64` string.

![](/assets/img/posts/ctf/patriotctf2023/cmulti-numeral5.png)

After decoding it we have our flag.

![](/assets/img/posts/ctf/patriotctf2023/cmulti-numeral6.png)


`PCTF{w0w_s0_m4ny_number5}`



## 3. **OSINT**

### a. Bad Documentation

![](/assets/img/posts/ctf/patriotctf2023/obd1.png)

We are given a github repo in which a security researcher leaked his password.

Opening the repo, I found 8 commits. I browsed around the history points of the commits till I found this: `https://github.com/Th3Burn1nat0r/vuln/blob/6823219585c693c9650c0dda545b0e93a5e4d554/J-Link/JLE25006.png`

![](/assets/img/posts/ctf/patriotctf2023/obd2.png)

I saw an Authorization header using Basic which utilizes base64 encoding.
Decoding it, we see the plain password.

![](/assets/img/posts/ctf/patriotctf2023/obd3.png)


`PCTF{N0_c0D3's_3VeRy_R3aLlY_G0n3}`



## 4. **Misc**

### a. WPA

![](/assets/img/posts/ctf/patriotctf2023/mwpa1.png)

We are given a `pcap file` which seems to have a 4 way `wifi handshake`. Let's proceed to decipher the wifi keys with `aircrack-ng`.

![](/assets/img/posts/ctf/patriotctf2023/mwpa2.png)

That was first!

`PCTF{qazwsxedc}`



### b. Twins

![](/assets/img/posts/ctf/patriotctf2023/mtwins1.png)

We are give two files: `he.txt` and `she.txt`. They seem to have the same content but `diff` showed some difference.

![](/assets/img/posts/ctf/patriotctf2023/mtwins2.png)

I uploaded to [diff checker](https://www.diffchecker.com/) and checked out the differences.

![](/assets/img/posts/ctf/patriotctf2023/mtwins3.png)

I wrote down the unique characters from the highlighted words to get the flag. 
Alternatively, pasting both file contents in `cyberchef` and separating them by 4 lines gives the flag faster.

![](/assets/img/posts/ctf/patriotctf2023/mtwins3b.png)

`PCTF{4wes0M3_sTories_man}`



---

I had fun solving these and more challenges from the many challenges in PatriotCTF2023. As I was learning I explored several ways of solving them.

There are many ways of killing a rat! Till next time.

Happy Hacking.