
# SheHacksKE KCA Intervarsity CTF — 2022 Write up

A short write up for some ctf challenges held at KCA University during the intervarsity bootcamp by SheHacksKE, fr334aks, Safaricom, AfricaHackOn, Ekraal Innovation Hub, Trend Micro & Microsoft.

Challenge link [https://kca.ciphercode.dev/](https://kca.ciphercode.dev/)

![](https://cdn-images-1.medium.com/max/2000/1*S0c_Kg2yztHU_7QzdmxsMw.png)

## OSINT
## Challenge: Hunter1

Given a SHA256 hash of a malicious office document,
What’s the name of the ransomware behind the Malicious Office Document?

![](https://cdn-images-1.medium.com/max/2000/1*q8uUj_15V677nDChFy1mcA.png)

This was pretty easy.

### Solution

Searching the SHA256 hash on Google brings up a link highlighting “Gandcrab ransomware” and that’s it, you have hunted down the name of the ransomware. flag{gandcrab}

## Challenge: Last Hunt

This hunt was about the payment methods used by gandcrab.
Other than BTC the Ransomware group also uses?

![](https://cdn-images-1.medium.com/max/2000/1*ZJ1-g4-Pxs5B0PAbUAhwUw.png)

### Solution

I did some googling on “gandcrab ransomware” and went through articles.

![](https://cdn-images-1.medium.com/max/2000/1*Wu7edNSyipaTSKP5il3epA.png)

A story featured on [malwarebytes](https://www.malwarebytes.com/gandcrab) [(here)](https://www.malwarebytes.com/gandcrab) stated the payment method.

![](https://cdn-images-1.medium.com/max/2040/1*zNyQ5DDRDlPv908jxuDlPA.png)

flag{dash}

## Forensics
## Challenge: Chatty Chatty — 2

“See ye and you shall find me”. Dig deeper to earn the second flag.
I didn’t get the first flag but here, I could “see and find”

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

There are many ways of killing a rat!

Connect with me: [Here](https://d-captainkenya.github.io/)

Happy Hacking.
