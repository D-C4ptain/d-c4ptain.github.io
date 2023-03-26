
# End Of The Year UDOM CTF 2022

Just a few hours before “cd year-2023” or “sudo apt install year-2023” if you’d prefer, I participated in an awesome CTF competition organized by UDOM CYBER CLUB.

![](https://cdn-images-1.medium.com/max/2160/1*7heGcshZw5UTZOJpBOQMAA.jpeg)

I solved a few challenges ranging from: OSINT, Forensics, Cryptography, Steganography to Web. Let’s take a look.

## 1. Cryptography

### Challenge: R54

![](https://cdn-images-1.medium.com/max/2000/1*c_eniZIdY2b1feSHu52iCw.png)

We are given a ct.txt file and a challenge.py file that is supposed to help us get the flag. This is what was in the files.

![ct.txt](https://cdn-images-1.medium.com/max/2000/1*xpDOu23tM6o6GWSJXiG0RQ.png)*ct.txt*

![challenge.py](https://cdn-images-1.medium.com/max/2000/1*DnD8AwSobdUK_CqLzJVmvw.png)*challenge.py*

As you know RSA algorithm is an asymmetric cryptography algorithm. Asymmetric simply means it works on two different keys i.e. Public Key and Private Key. One key is used for encryption while the other is used for decryption. If one key is used to encrypt data, the other key decrypts, and vice versa. This ensures data confidentiality and integrity(depending with which key you start with). RSA is usually based on exactly two prime numbers.

![](https://cdn-images-1.medium.com/max/2000/1*YPdQiDcn3PMn6hVCxnBC_Q.jpeg)

Okay, So I went ahead to use the python challenge file and landed on some “modulus invertibility error” which I did not want to debug(It’s end of year, Gotta be somewhere making resolutions)

I tried an online rsa deciphering site([here](https://www.dcode.fr/rsa-cipher)). From the ct file we have n, e, and ct. In RSA: n is usually the modulus, e is the encryption exponent: ct was our cypher message.

![](https://cdn-images-1.medium.com/max/2000/1*NU9Q8Cay2zxaQrPms4dPuA.png)

Feeding this details on the site gave the flag: flag{usa_e_piu_grandi}

## 2. OSINT

### Challenge: Place

![](https://cdn-images-1.medium.com/max/2000/1*dW0i4hs2n1CUATUS0Mb_EA.png)

We are given an image file “maporomoko.jpg” 
Headed over to [https://tineye.com/](https://tineye.com/) and did an image reverse search.

![](https://cdn-images-1.medium.com/max/2000/1*K4d_5rQPEwrVcoFOAEYnYA.png)

Got the name of the waterfall. Googled first result out “Kasanga_Rukwa Region”

![](https://cdn-images-1.medium.com/max/2000/1*-QOhtUzxpj1GAy_Ovof7PA.png)

And Google did the rest: UDOM{Rukwa}

## 3. Forensics

### Challenge: file

![](https://cdn-images-1.medium.com/max/2000/1*_o79F-krcJCcT-Y2XXeHfw.png)

We have flag.zip file that I extracts to myfile.pdf

![](https://cdn-images-1.medium.com/max/2000/1*qWTDeH10CSbL0Seh1iLLng.png)

While expecting something, opening the file as pdf was not possible.

![](https://cdn-images-1.medium.com/max/2000/1*yeOX6g6Pj6Xgl6dwTHqNmQ.png)

I decided to confirm the file type which turned to be an audio file.

![](https://cdn-images-1.medium.com/max/2000/1*dLETG0z5Pf4X1n-7vulW2Q.png)

Opened it with [Audacity](https://www.audacityteam.org/) only to find myself dancing to morse code.
Quickly uploaded the file to an online morse code decoder. [Link here](https://morsecode.world/international/decoder/audio-decoder-adaptive.html)

![](https://cdn-images-1.medium.com/max/2000/1*P63_PmLCnIuccvTa0SfekQ.png)

I replayed a few times just to be sure. UDOM{**M0RS3S0UND5B3TT3R1NMIL1TAR13S**}

## 4. Steganography

### Challenge 1: Starting point

![](https://cdn-images-1.medium.com/max/2000/1*eBIQlsQc0ZI8Eu5wvEev1w.png)

We are given a pdf file which when opened, we see:

![](https://cdn-images-1.medium.com/max/2000/1*0jVhXNbxZKrivXm38tfVwQ.png)

![We say ‘buda’ in Kenya, hehe](https://cdn-images-1.medium.com/max/2000/1*myzkv7TiOWOfK35oG_JOYA.png)*We say ‘buda’ in Kenya, hehe*

Tried using steghide to extract hidden files but was prompted with a passphrase. Trying a simple brute force with pdfcrack suggested no encryption.
After running around, I found this site([here](https://products.aspose.app/pdf/parser/pdf)), uploaded the file and checked the parsed files.

![](https://cdn-images-1.medium.com/max/2000/1*22dvJuINPvKlAky9MbijQg.png)

### Challenge 2: Simple stego

![](https://cdn-images-1.medium.com/max/2000/1*nfvQQ1egOCgeVzV3IHod7g.png)

This CTF also featured WindowsXP as a png file.
Viewing the image shows some blurred markings on the top left corner.

![](https://cdn-images-1.medium.com/max/2000/1*Tq5cXt1vx1PmUx2asLq33w.png)

Zooming in brings it better.

![UDOM{BR1LL14NT_5T4RT5_H3R3}](https://cdn-images-1.medium.com/max/2000/1*xsoqkdka4-FqF5ABtkeEIA.png)*UDOM{BR1LL14NT_5T4RT5_H3R3}*

## 5. Web

### Challenge 1: Turaco

![](https://cdn-images-1.medium.com/max/2000/1*rwUpwH9f8KzRYhGOxxvxPg.png)

We have a site which shows nothing when visited. I checked the “page source” and found this.

![](https://cdn-images-1.medium.com/max/2000/1*rHzub5rYOpf29J1XHRteyg.png)

Decrypted the decimal values in pass variable only to be told:
“Uo ni uwongo bhanaa uwongo” — That’s not true men!

I just learnt ‘password’ in Swahili is called msimbo or is it sambo?(It’s msimbo; Thanks to [Stuxkyle](https://twitter.com/stuxkyle8))
There are sambo values in hex down here.
Pasting the hex from the function to [Cyberchef](https://gchq.github.io/CyberChef/) we get decimals that when added a decimal recipe give us the flag.

![UDOM{1ts_an_0m3n}](https://cdn-images-1.medium.com/max/2000/1*LpDNjV8AweLIJbz6-ikSbg.png)*UDOM{1ts_an_0m3n}*

Came back later on to find the CTF had ended!

### Challenge 2: Two-step-snake

![](https://cdn-images-1.medium.com/max/2000/1*DxPZGFkoIEl38cWOfQC0rw.png)

We have a site which when visited shows nothing.

Viewing the source code reveals a url encoded string within a ‘unescape’ function.

Did a double url decode with [cyberchef](https://gchq.github.io/CyberChef/) and decrypted the resulting decimal values to get the password.

The CTF was really awesome though didn’t get enough time for it.

![](https://cdn-images-1.medium.com/max/2000/1*Z9vJ0eQPWu-4m4unkx0dPw.png)

Oh! and honestly, there are many ways of killing a rat!

[Let’s connect here](https://d-captainkenya.github.io/).

Happy Hacking.
