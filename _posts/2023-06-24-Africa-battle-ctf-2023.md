---
title: Africa Battle CTF 2023 Write up
author: D_C4ptain
date: 2023-06-24 12:33:00 +0800
categories: [CTF, write up]
tags: [Africa, Africa battle, write up, walkthrough, ctf, web, forensics, d_captain, d_c4ptain, python, server side template injection, ssti, whatweb, cyberchef, base64, exiftool, wireshark]
math: true
mermaid: true
image:
  path: /assets/img/posts/ctf/africabattle23/africabattlectf23.png
---
---

This weekend I Participated in Africa Battle CTF 2023. It was an awesome learning experience, thanks to the organizers.  
These are just but a few challenges I made this write up for.

## 1. Forensics

### a. Thumb

![](/assets/img/posts/ctf/africabattle23/fthumb1.png)

We are given a `file.jpeg` file with a "script kiddie" who wishes us all the best."

![](/assets/img/posts/ctf/africabattle23/fthumb1b.jpeg)

I ran `strings` on the image but found nothing of interest. I used [cyberchef](https://gchq.github.io/CyberChef/) to extract files and landed on a QR code.

![](/assets/img/posts/ctf/africabattle23/fthumb2.png)

I scanned the code and there she was waiting to be submitted.

![](/assets/img/posts/ctf/africabattle23/fthumb3.png)


### b. Find Me

![](/assets/img/posts/ctf/africabattle23/ffindme1.png)

I ran `strings` on the `pcap file` and found credentials which seemed to be for a router's log in page.

![](/assets/img/posts/ctf/africabattle23/ffindme1b.png)


![](/assets/img/posts/ctf/africabattle23/ffindme2.png)

The password string seemed to be `url encoded` so I decoded it and got a `base64` string.

![](/assets/img/posts/ctf/africabattle23/ffindme3.png)

Decoding the string from base64 revealed the plain text password.

![](/assets/img/posts/ctf/africabattle23/ffindme4.png)

Alternatively, I opened the pcap file with wireshark. I filtered for http traffic and checked the post request.  
I got the creds and proceeded to decode the password string from base64.

![](/assets/img/posts/ctf/africabattle23/ffindme5.png)


### c. Africa Beauty

![](/assets/img/posts/ctf/africabattle23/fafbeauty1.png)

We are given a `zip file` which extracts to an image of a beautiful african art.

![](/assets/img/posts/ctf/africabattle23/fafbeauty1b.jpg)

I checked the image metadata with `exiftool` and sure enough found everything I needed.

![](/assets/img/posts/ctf/africabattle23/fafbeauty2.png)


![](/assets/img/posts/ctf/africabattle23/fafbeauty3.png)


![](/assets/img/posts/ctf/africabattle23/fafbeauty4.png)

Exiftool really can be helpfull. I proceeded with the gps coordinates to get the location.

![](/assets/img/posts/ctf/africabattle23/fafbeauty5.png)


![](/assets/img/posts/ctf/africabattle23/fafbeauty6.png)


## 2. Web

### a. Cobalt Injection

![](/assets/img/posts/ctf/africabattle23/wcinjection1.png)

I found nothing of interest on opening he site.

![](/assets/img/posts/ctf/africabattle23/wcinjection2.png)

I checked the `page source` and got a comment about an endpoint.

![](/assets/img/posts/ctf/africabattle23/wcinjection3.png)

I tried accessing it out but turned to be a `404`

![](/assets/img/posts/ctf/africabattle23/wcinjection4.png)

I striped the 'IP' string and the "Benin" string was actually reflected on the page! That is now interesting.

![](/assets/img/posts/ctf/africabattle23/wcinjection5.png)

I checked the tech stack with `whatweb` which revealed the app was python based.

![](/assets/img/posts/ctf/africabattle23/wcinjection6.png)

I tried different countries but it didn't work.  
I went for `XSS` tried a few things here and there but got nothing I decided to try later.

![](/assets/img/posts/ctf/africabattle23/wcinjection7.png)

Later on after the ctf was over, a [friend](https://twitter.com/byronchris25) approached it with `server side template injection` techniques.  
[Server Side Template Injection(SSTI)](https://portswigger.net/web-security/server-side-template-injection) - Allows an attacker to include template code into an existing (or not) template. A template engine makes designing HTML pages easier by using static template files which at runtime replaces variables/placeholders with actual values in the HTML pages.

I learned that the app was built on [Jinja2](https://jinja.palletsprojects.com/en/3.1.x/) - a templating engine for python web apps: django and flask.

I had to come back and finish what I started!  
I checked on [`PayloadsAllTheThings`](https://github.com/D-C4ptain/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#jinja2---basic-injection) and tried a basic Jinja payload.

![](/assets/img/posts/ctf/africabattle23/wcinjection8.png)

Wait, it actually gave me the product of 4! 

![](/assets/img/posts/ctf/africabattle23/wcinjection9.png)

I tried another payload to dump the configuration variables of the app and here they were!

![](/assets/img/posts/ctf/africabattle23/wcinjection10.png)

I went on and tried reading the `passwd` file.

![](/assets/img/posts/ctf/africabattle23/wcinjection11.png)

I tried `directory listing` and saw the flag file.

![](/assets/img/posts/ctf/africabattle23/wcinjection12.png)

I went for another payload and read the flag.

![](/assets/img/posts/ctf/africabattle23/wcinjection13.png)


---

I learnt alot from the many challenges in this ctf.

There are many ways of killing a rat!

Happy Hacking.
