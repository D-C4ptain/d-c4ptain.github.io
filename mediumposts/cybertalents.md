
# 2022 CyberTalents Bootcamp CTF Writeup

Let’s take a look at some challenges from the lessons previewed.
https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022

## **Challenge 1: mask(Network Security Fundamentals)**

[https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/network-security-fundamentals/challenges/mask](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/network-security-fundamentals/challenges/mask)

**What is the subnet mask for this host?**

![](https://cdn-images-1.medium.com/max/2000/1*U-Wdx8Lvz_bU72tNIoXi4w.png)

flag format: xxx.xxx.xxx.x

**Description
**In Windows, ***ipconfig* **is a console application designed to run from the Windows command prompt. This utility allows you to get the IP address information of a windows computer. It also allows some control over your network adapters, IP addresses (DHCP-assigned specifically), even your DNS cache. From the terminal we can see the subnet mask of the computer.

## **Solution**
> 255.255.255.0

## **Challenge 2: anonymous(Wireshark)**

[https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/wireshark/challenges/anonymous](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/wireshark/challenges/anonymous)

Can you trace the anonymous guy?

![](https://cdn-images-1.medium.com/max/2012/1*HBlEJjVxnXdEk1eFg-JLdQ.png)

## **Description**

Go ahead and download the ‘pcap’ file from the description link.
Open the file with Wireshark.

![](https://cdn-images-1.medium.com/max/2360/1*w9q_moKQiNglacWfN_RXBQ.png)

From tabs(top), go to Statistics, Protocol Hierarchy.

![](https://cdn-images-1.medium.com/max/2716/1*2LSbhnyjupQqa3fPBYFg_g.png)

Right click on ‘Data’ and select ‘apply as filter’, ‘selected’

![](https://cdn-images-1.medium.com/max/2000/1*MRDH3Stdga8zmd2cqZAmkg.png)

Right click on second packet and ‘Follow’, ‘TCP stream”

![](https://cdn-images-1.medium.com/max/2650/1*x99J47OqziYlOv-UhMC9uA.png)

Decode the string that appears from base64.
I really like CyberChef.

![](https://cdn-images-1.medium.com/max/2046/1*u3Kv3nzR8XeLLChAOYEtxQ.png)

And somehow you have your anonymous flag.

## **Challenge 3: Hide data(Data Encryption)**

[https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/network-security-fundamentals/challenges/hide-data](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/data-encryption/challenges/hide-data)

![](https://cdn-images-1.medium.com/max/2000/1*fxUpUgIEpqCngdGC_Baxeg.png)

### **Description**

I used to hide my data with a classic cypher, can you get the flag hidden inside? 
gur synt vf 2w68lsudym Vg vf cerggl rnfl gb frr gur synt ohg pna lbh frr vg v gbbx arneyl 1 zvahgr gb rapbqr guvf jvgu EBG13 tbbq yhpx va fbyivat gung

### **Solution**

Copy paste the string into [cyberchef](https://gchq.github.io/CyberChef/).
Use ROT13 recipe and bake for this delicious flag.

![](https://cdn-images-1.medium.com/max/2658/1*J6Meom1xiizD05vdVUrJOQ.png)

And there you have your flag.

## **Challenge 4: Keep ASCIIng(Data Encoding)**

[https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/data-encoding/challenges/keep-asciing](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/data-encoding/challenges/keep-asciing)

![](https://cdn-images-1.medium.com/max/2000/1*MjIIpxiCS3Oy4KaykkgWQg.png)

### **Description**

One form of string representations is readable but the other is not. can you read the flag. 102108097103123067084095049115116069097115121083051099114051116051080064115115112104114052053051125
Flag format is FLAG{XXXXXXXXXX}

### **Solution**

For ASCII I simply went [HERE](https://www.dcode.fr/ascii-code)

![](https://cdn-images-1.medium.com/max/2000/1*7TS-fOxUw6j7OwkByXoUzA.png)

And there we go, our flag is right there, not on the right though.

## Challenge 5: ADAMIN(Network Protocols)

h[ttps://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/network-protocols/challenges/adamin](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/network-protocols/challenges/adamin)

![](https://cdn-images-1.medium.com/max/2000/1*Zo_F0gSucM5tdivnXSvCLg.png)

### Description

Download the pcap file and open it with wireshark. I looked at the udp packets but nothing cool about them stuff is in ftp. Filter the packets to ftp and follow along.

![](https://cdn-images-1.medium.com/max/2104/1*5Os8yreqWfQH35LnJdLXFA.png)

You have seen packets 875 and 879. They give use a username: ADAM and a password: oneforallpass, try not to forget what you see. Let’s check for some data through the ftp. Filter the packets further with “ftp-data”

![](https://cdn-images-1.medium.com/max/2000/1*oLOFZkcU1hkGWbVLrJ6UIA.png)

Interesting. Let’s check the packet with some zip file and follow it’s tcp stream.

![](https://cdn-images-1.medium.com/max/2442/1*-oTenqawbD8hkvChMNntZw.png)

Change the ASCII data(show data as) into raw and save it(save as). Now extract the zip file you saved.

tar will do or use ‘7z x zipfilename’ if you have 7z. When prompted for a password remember to remember what you saw earlier.

![](https://cdn-images-1.medium.com/max/2000/1*_E3JUA3D8EFdpCH036laiw.png)

Listing the directory shows a ‘sheets’ directory, heading there we find another compressed file.

Let’s decompress it with: 7z X Salaries.csv.tar.xz
We get Salaries.csv.tar
Let’s decompress it again: 7z X Salaries.csv.tar

Listing the directory we see a ‘salaries.csv’ file which doesn’t seem to have any flag.
List the directory once more and this time with ‘ls -al’
‘la’ does it for me too.

Notice and open that hidden txt file over there and get your flag.

![](https://cdn-images-1.medium.com/max/2000/1*2NUWYwv86ZNHWmRiY-EJfA.png)

### Solution

flag{ follow along }

## Challenge 6: This is Sparta(Javascript)

[https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/javascript/challenges/this-is-sparta](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/javascript/challenges/this-is-sparta)

![](https://cdn-images-1.medium.com/max/2000/1*aeeMhFoOFvYgsO52wx0X2g.png)

### Description

Starting the challenge we head to a login page. The hint seems to just encourage us.
Let us view the page source.

![](https://cdn-images-1.medium.com/max/2270/1*Baiz6XXxaSCSdAxazpeawg.png)

We see some weird(obfuscated) javascript down there. Copy the code in between the script tags and paste it [here](https://deobfuscate.io/) as the input. Let us deobfuscate it.

![](https://cdn-images-1.medium.com/max/2368/1*KK57g7kElDGuHh2muGMMEg.png)

Inspecting the output we can clearly see that the Js code is checking if the username and password entered is ‘Cyber-Talent’ before granting access.
Let’s go ahead and login.

![](https://cdn-images-1.medium.com/max/2000/1*JrgaYSszY6-3YLedqbuNFQ.png)

On logging in we get our flag.

### Solution

Where the curly braces begin.

## **Challenge 7: Admin has the power(Cookies)**

[https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/cookies/challenges/admin-has-the-power](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/cookies/challenges/admin-has-the-power)

![](https://cdn-images-1.medium.com/max/2000/1*MIqpe0il0G0RrUqJgHCLww.png)

### Description

Starting the challenge we are redirected to a login page

![](https://cdn-images-1.medium.com/max/2000/1*9E2c-lgw8KzAU4P_hKNBjQ.png)

Let’s see the page source for this page.

![](https://cdn-images-1.medium.com/max/2000/1*58JonYO373BB4D63kZfOuQ.png)

Nice, The developer left some credentials for maintenance purpose. Log in using those creds.

The site welcomes us as support but we really need the powers of the admin.
Looking at the cookies, we have a cookie named ‘role’ with a value ‘support’. Let’s change the value to ‘admin’ and refresh the site.

![](https://cdn-images-1.medium.com/max/2390/1*L95mAdBqzD8Un047JwhCJg.png)

![](https://cdn-images-1.medium.com/max/2110/1*LP_rYLU6EqO3J0Pmbh18Jg.png)

### Solution

Refreshing the site we are given the power to see the flag.

![](https://cdn-images-1.medium.com/max/2110/1*yJ17Vj_EHVT1-QvFf6jAGg.png)

## **Challenge 8: http(OSI Model)**

[https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/osi-model/challenges/http](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/osi-model/challenges/http)

![](https://cdn-images-1.medium.com/max/2000/1*WHvMlhkzTBP1vGA0u14V3Q.png)

### Description

Start the server and login.

![](https://cdn-images-1.medium.com/max/2000/1*RVpckK_pRVNDuuNvAUkZ7A.png)

Use curl on ctweb.com

### Solution

Going through the html response, we have a fl4g.html describe at the title tags. Curl through the page and get your flag.

![](https://cdn-images-1.medium.com/max/2000/1*Rwade80TQY2wK2AUaBtqmw.png)

You could also check the root directory on the server as follows.

![](https://cdn-images-1.medium.com/max/2000/1*fhzYR52T003R9ZlbaMyx3Q.png)

## **Challenge 8: I love music(Steganography)**

[https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/steganography/challenges/i-love-music](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/steganography/challenges/i-love-music)

![](https://cdn-images-1.medium.com/max/2000/1*qRyeEDbnL0fjvhnJEtIovA.png)

### Description

Download the wav file and open it with audacity.
If you don’t have it installed run ‘sudo apt update’ and sudo apt install audacity’

![](https://cdn-images-1.medium.com/max/2566/1*fOcQ3bLWIoYtJh9j1BmN0g.png)

Change the view to ‘spectogram’ from the highlighted section.

![](https://cdn-images-1.medium.com/max/2000/1*7YqFlDoZIkEUot_074NwPQ.png)

### Solution

There you go.

![](https://cdn-images-1.medium.com/max/2308/1*IehpzHwEFsXH03iRzvtYkQ.png)

## **Challenge 9: Images3c(Steganography)**

[https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/steganography/challenges/images3c](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/steganography/challenges/images3c)

![](https://cdn-images-1.medium.com/max/2000/1*iTX_lF7DBLrObftHnPxXGA.png)

### Description

I checked the image with exiftool and found nothing. I tried extracting hidden files with steghide which prompted for a passphrase.

![](https://cdn-images-1.medium.com/max/2000/1*u_fB3Fgri66ju7zDY3jBkA.png)

Let’s crack the passphrase with stegcracker.

![](https://cdn-images-1.medium.com/max/2000/1*ZHGvLynbU28taKxXlfcDeQ.png)

### Solution

We have a cyber.jpg.out file let’s check on it.

![](https://cdn-images-1.medium.com/max/2000/1*NvzFnpm0czXN9WoYoeXJWQ.png)

## **Challenge 10: Back to basics(Web Application Basics)**

[https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/web-application-basics/challenges/back-to-basics](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/web-application-basics/challenges/back-to-basics)

![](https://cdn-images-1.medium.com/max/2000/1*2GQcOtxJRneb59hf5z7JpA.png)

### Description

Tried curl on the link but got nothing.

![](https://cdn-images-1.medium.com/max/2000/1*2a1kSDmuO_D-wBnj8izuiA.png)

Different servers can have custom methods apart from the usual: GET, POST, PUT, DELETE etc.
Let us see what this one supports.

![](https://cdn-images-1.medium.com/max/2000/1*vcSNyTIf_WpoiXjMtdK-bQ.png)

Down there we see the available options. Using GET gives us nothing.
Let us use P0ST. Notice the 0 and not O.

![](https://cdn-images-1.medium.com/max/2030/1*Y4w1y6y8Ebsu_BhNPCvdzg.png)

Notice the extra output. Looks like some obfuscated Javascript. Let’s deobfuscate it [here](https://deobfuscate.io/).

![](https://cdn-images-1.medium.com/max/2000/1*Lmjt5NF_HpZR8Sdq0E358w.png)

Here is our flag but it seems to be reversed by the Js function.

### Solution

Let us reverse it to the original flag with some python.(You can use any other means.)

![](https://cdn-images-1.medium.com/max/2000/1*YrEXBB7wOYtAXXOtuvw4vg.png)

Save as python file and execute it to get original flag.

![](https://cdn-images-1.medium.com/max/2000/1*-A_oIdLRT7_ZSfOmEQfkOw.png)

## **Challenge 11: Cheers(PHP Basics)**

[https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/php-basics/challenges/cheers](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/php-basics/challenges/cheers)

![](https://cdn-images-1.medium.com/max/2000/1*Lm-TZChpWdZZIyEnqT5QNg.png)

### Description

Starting the challenge leads us to a broken index page.

![](https://cdn-images-1.medium.com/max/2000/1*WVJFdhS492NEJvXeWtfC9Q.png)

I tried ‘flag.txt, flag.php and /welcome.php’ end points but got 404s.
I tried ‘/?welcome’ and got some cool response.

![](https://cdn-images-1.medium.com/max/2000/1*9Jzam7lQyA8lNB9YybATpg.png)

### Solution

I tried ‘/?gimme_flag’ and I was back to square one.
Then I tried this ‘/?welcome&&gimme_flag’ and boom!

![](https://cdn-images-1.medium.com/max/2000/1*GG7uyAC1nSNpQi_1qfjHpQ.png)

Nice warm up.

## Challenge 12: remote-CVE(Vulnerability Assessment)

[https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/vulnerability-assessment/challenges/remote-cve](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/vulnerability-assessment/challenges/remote-cve)

### Description

What’s CVE ID could be used against the web application in the below target

![](https://cdn-images-1.medium.com/max/2000/1*8tBfx9RRm0GBKWchfJublw.png)

I opened the IP with Firefox but could not get a connection.
I scanned the IP with nmap to check for all open ports.

| sudo nmap -sV -p- 3.71.47.24 -Pn -T4 -v

![](https://cdn-images-1.medium.com/max/2000/1*NHLnJPBjmv_XZ9GECkn3zQ.png)

Patience pays. After a couple of seconds I got what I needed.

![](https://cdn-images-1.medium.com/max/2000/1*26pvuIhXNjAXYy-mG6LUIg.png)

Did some googling on the first http server at port 1337: ‘HttpFileServer httpd 2.3’ and found numerous results.

![](https://cdn-images-1.medium.com/max/2000/1*hB4GyHkiVjP-aADyOBeGVQ.png)

I checked out the first result.

![](https://cdn-images-1.medium.com/max/2442/1*8oDW46WD_kpGBIc7DcjZQw.png)

There I got the CVE number and the vulnerability’s exploit.

Flag format CVE-xxxx-xxxx

## Challenge 13: WPA Crack (WIFI Attacks)

[https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/wifi-attacks/challenges/wpa-crack](https://cybertalents.com/learn/intro-to-cybersecurity-bootcamp-2022/wifi-attacks/challenges/wpa-crack)

### Description

You are conducting a WIFI pentest, Handshake has been captured and your task is to crack it

Flag format is just the password

![](https://cdn-images-1.medium.com/max/2000/1*ODQP2mZqUEVk7xJBqhnHfg.png)

This was quit easy.
I downloaded the cap file and used aircrack-ng with ‘rockyou’ for dictionary attack.

![](https://cdn-images-1.medium.com/max/2000/1*7YhQXdosjf-oiIhfbfxfNA.png)

![](https://cdn-images-1.medium.com/max/2000/1*Sm7Fy_dusn6v6fZMZXuruw.png)

And there we got the password.

There are many ways of killing a rat!

Connect with me: [Here](https://d-captainkenya.github.io/)

Stay Hacking.
