
# h4cked — Tryhackme Detailed Writeup

Find out what happened by analyzing a .pcap file and hack your way back into the machine
Room link here

## Task 1 — Oh no! We’ve been hacked!

It seems like our machine got hacked by an anonymous threat actor. However, we are lucky to have a .pcap file from the attack. Can you determine what happened? Download the .pcap file and use Wireshark to view it.

![](https://cdn-images-1.medium.com/max/2000/1*8L4kuDZBj_m8BblmCr-YWQ.png)

### >> The attacker is trying to log into a specific service. What service is this?

Looking around the file, I noticed brute force-like traffic on some protocol.

![](https://cdn-images-1.medium.com/max/2000/1*p3Z7OpPW6R_5N3Lap7WgKQ.png)

ANS [protocol name]

### >> There is a very popular tool by Van Hauser which can be used to brute force a series of services. What is the name of this tool?

Just google this out.

### >> The attacker is trying to log on with a specific username. What is the username?

There’s a repeated username somewhere.

### >> What is the user’s password?

Let’s filter the service.

![](https://cdn-images-1.medium.com/max/2000/1*4Wz0_wvJAjKtrqYppa4DRg.png)

Checking around, we can see several failed login tries. There’s however, a successful login.

![](https://cdn-images-1.medium.com/max/2000/1*FRwaQazhbXgXx9RGWhvdjw.png)

Let’s follow it’s tcp stream, there you go.

![](https://cdn-images-1.medium.com/max/2000/1*GdmnQOpwl-m17kmuZ4JvUQ.png)

There’s also another “login successful” packet. Above it, there is the password.

![](https://cdn-images-1.medium.com/max/2000/1*u0wYBWY3-OftV4xgsa8uuA.png)

### >> What is the current FTP working directory after the attacker logged in?

Let’s follow the tcp stream of a packet after a successful login. The attacker executes ‘PWD’ command to find out their current working directory.

![](https://cdn-images-1.medium.com/max/2000/1*cK05vHQwGgI7u1OwjZOk2w.png)

### >> The attacker uploaded a backdoor. What is the backdoor's filename?

It’s clearly right there from the tcp stream.

![](https://cdn-images-1.medium.com/max/2000/1*L_hNkBEQ0M3BKj45v1lR3Q.png)

### >> The backdoor can be downloaded from a specific URL, as it is located inside the uploaded file. What is the full URL?

We need to check in the backdoor file, Let’s filter for “ftp-data”

![](https://cdn-images-1.medium.com/max/2000/1*ir0NAqbxr8NyORL2E6lnEQ.png)

Follow the tcp stream of the file’s packet or check the ‘line based text data’ on the packet details pane.
Scroll around you’’ll see the url

![](https://cdn-images-1.medium.com/max/2000/1*regZl8G4FRupQIoFUiWK_Q.png)

### >> Which command did the attacker manually execute after getting a reverse shell?

Follow tcp stream of a packet after a request to the shell.

![](https://cdn-images-1.medium.com/max/2000/1*CQYR4XUyWIpZv3j97aV1yg.png)

### >> What is the computer’s hostname?

Refer the above stream for system information.

### >> Which command did the attacker execute to spawn a new TTY shell?

From the stream, the attacker uses a simple python oneliner to stabilize the reverse shell.

![](https://cdn-images-1.medium.com/max/2000/1*61SNsoHBeHtgWwsxaCgn9w.png)

### >> Which command was executed to gain a root shell?

Just check the stream where the attacker is now root.

![](https://cdn-images-1.medium.com/max/2000/1*taZVQdC4V4Lf1x3-et91nQ.png)

### >> The attacker downloaded something from GitHub. What is the name of the GitHub project?

Scrolling down, we see a git clone of some github repository.

![](https://cdn-images-1.medium.com/max/2000/1*NSN5yK_Er62mJNUUXFGVFg.png)

### >> The project can be used to install a stealthy backdoor on the system. It can be very hard to detect. What is this type of backdoor called?

Google about this backdoor.

## Task 2— Hack your way back into the machine

That was some nice blue team exercise, let’s go red!

Deploy the machine.

The attacker has changed the user’s password! Can you replicate the attacker’s steps and read the flag.txt? The flag is located in the /root/Reptile directory. Remember, you can always look back at the .pcap file if necessary. Good luck!

### >> Run Hydra (or any similar tool) on the FTP service. The attacker might not have chosen a complex password. You might get lucky if you use a common word list.

A simple scan reveals ftp on port 21.

![](https://cdn-images-1.medium.com/max/2000/1*sit2bl3ds9I8MBhqxdxVVA.png)

Let’s brute force ftp with hydra:

    hydra -l jenny -P /usr/share/wordlists/rockyou.txt ftp://10.10.115.31

![](https://cdn-images-1.medium.com/max/2000/1*3kfyKlK_zbKf2UiFZjF9fA.png)

That was fast. We can now log in ftp.

### >> Change the necessary values inside the web shell and upload it to the webserver.

Download the attacker’s shell.

![](https://cdn-images-1.medium.com/max/2000/1*cEJiL0_J1uinDRe9GQS8aw.png)

Let’s change the IP and PORT to our own.

![](https://cdn-images-1.medium.com/max/2000/1*9CX53JAQN31n5RdJME2v7w.png)

Confirm your own IP address and use the tryhackme VPN(tun0) IP.
Let’s upload the shell to ftp. (you may need to re-login).

![](https://cdn-images-1.medium.com/max/2000/1*gITTwqZ5LvarJldujsY-Xg.png)

### >> Create a listener on the designated port on your attacker machine. Execute the web shell by visiting the .php file on the targeted web server.

Let’s listen for the reverse shell with netcat(use the PORT you specified).

    nc -lnvp 1337

visit the shell from the web server.

![](https://cdn-images-1.medium.com/max/2000/1*m_LjZj9M-e_6fXiIGS0S2A.png)

And we get a shell.

![](https://cdn-images-1.medium.com/max/2144/1*ZCraoh_vaYrskcjnKBldhA.png)

### >> Become root!

Let’s first stabilize our rev shell with:

    python3 -c 'import pty;pty.spawn("/bin/bash")'

We then switch to user: jenny
Looking at “sudo -l”, we see jenny can run all sudo commands.

![](https://cdn-images-1.medium.com/max/2000/1*LDJzjsfIs_bB60mlCDeqFA.png)

Switching to root with “sudo su” we get root.

![](https://cdn-images-1.medium.com/max/2000/1*2prWbEbV3sC-d6Ib2UJFzA.png)

### >> Read the flag.txt file inside the Reptile directory

Let’s find the ‘Reptile’ directory with:

    find / -type d -name Reptile 2</dev/null

We have our flag right there!

![](https://cdn-images-1.medium.com/max/2000/1*wDpUt8Wkp4SsfdCxoKtLPw.png)

And we are done! Thanks to [toxicat0r](https://tryhackme.com/p/toxicat0r)

And by the way, There are many ways of killing a rat!

[Let’s connect here](https://d-captainkenya.github.io/).

Happy Hacking.
