
# REvil Corp — TryHackMe Writeup

You are involved in an incident response engagement and need to analyze an infected host using Redline.
Room link here
Difficulty: Medium

### **Investigating the Compromised Endpoint**

Open Redline Tool and load the Mandiant analysis file into Redline.
Alternatively, double click the Mandiant analysis file.

![](https://cdn-images-1.medium.com/max/2610/1*RUZe4idWIvMWxnHmbek1-Q.png)

### **1. What is the compromised employee’s full name?**

Take a look at ‘System Information’.

![](https://cdn-images-1.medium.com/max/2000/1*Sc-k9C_uU1Novl2KgYgj3A.png)

### **2. What is the operating system of the compromised host?**

Still under ‘System Information’

![](https://cdn-images-1.medium.com/max/2000/1*RJQdI7NhVbYMISmM7duVgw.png)

### **3. What is the name of the malicious executable that the user opened?**

From ‘File Download History’ some file was downloaded and definitely executed.

![](https://cdn-images-1.medium.com/max/2328/1*SFXKLG46jrd71dR-Fipi1g.png)

### **4. What is the full URL that the user visited to download the malicious binary? (include the binary as well)**

Refer image above and check ‘Source URL’

### **5. What is the MD5 hash of the binary?**

Let’s check all files from ‘File System’
Filter to only have ‘Downloads’ entries.

![](https://cdn-images-1.medium.com/max/2548/1*UGoWetkxGk7-2b4qrNxg-w.png)

You can spot our malicious binary, right?
Double click for more details on it.

![](https://cdn-images-1.medium.com/max/2042/1*dWKObKey3buhe0mPSM8_vA.png)

Killed that rat!

### **6. What is the size of the binary in kilobytes?**

It’s right up there!

### **7. What is the extension to which the user’s files got renamed?**

Adding more files in our filter reveals some desktop files with a weird extension.

![](https://cdn-images-1.medium.com/max/2032/1*9Cs_qJsDDRrfIgV0X6MAtQ.png)

Some weird extension name in Desktop entries.

### **8. What is the number of files that got renamed and changed to that extension?**

Head to ‘Timeline’ and see what files were renamed and changed.
Deselect the default filter and filter for ‘modified’ and ‘changed’ files.
In the search bar, search for files associated with the ‘.t48s39la’ extension.

![](https://cdn-images-1.medium.com/max/2148/1*kl53RORg5m6IjprLLgFW9A.png)

Check top right edge.

### **9. What is the full path to the wallpaper that got changed by an attacker, including the image name?**

This one took a while.
Searching for a ‘.bmp’ file

![](https://cdn-images-1.medium.com/max/2000/1*nE0rqeDEWP63Dfrw66iogg.png)

Searches always come back with something.

### **10. The attacker left a note for the user on the Desktop; provide the name of the note with the extension.**

There’s a reason why they call themselves Readme files.

![](https://cdn-images-1.medium.com/max/2000/1*RnJa5PK-HVbo0a2kLV9zlA.png)

### 11. The attacker created a folder "Links for United States" under C:\Users\John Coleman\Favorites\ and left a file there. Provide the name of the file.

Let’s check that folder.

![](https://cdn-images-1.medium.com/max/2156/1*9MoVCsjod--d7qTYiEglsg.png)

It must link to ‘USA’

### 12. There is a hidden file that was created on the user’s Desktop that has 0 bytes. Provide the name of the hidden file.

Applying our filter again to check the files in ‘Desktop’ we see some file of size 0 bytes.

![](https://cdn-images-1.medium.com/max/2174/1*hloAM9CCpTXTvUXlp9hjzw.png)

I knew you would find it too.

### 13. The user downloaded a decryptor hoping to decrypt all the files, but he failed. Provide the MD5 hash of the decryptor file.

I saw some ‘decryp…’ file somewhere.
Checked ‘Downloads’ folder but didn’t see it ,then ‘Desktop’

![](https://cdn-images-1.medium.com/max/2000/1*_sDgZOeBsCY8kROx82BOjQ.png)

![](https://cdn-images-1.medium.com/max/2000/1*G8BYh8qmKhZmVqo9whkeJQ.png)

### 14. In the ransomware note, the attacker provided a URL that is accessible through the normal browser in order to decrypt one of the encrypted files for free. The user attempted to visit it. Provide the full URL path.

Headed to ‘Browser URL History’ and looked for ‘decry…’

![](https://cdn-images-1.medium.com/max/2000/1*JVUmLz3cFJsB7XOxu439aQ.png)

Searches always work.

### 15. What are some three names associated with the malware which infected this host? (enter the names in alphabetical order)

This was quit easy.
Searched through [VirusTotal](https://www.virustotal.com/gui/home/upload) for our MD5 hash of the binary
And Checked through the community comments to find two common names.
Found the other name on [mitre attack website.](https://attack.mitre.org/software/S0496/)

![](https://cdn-images-1.medium.com/max/2000/1*bzP8bh1Slkwce7xLhg9nuQ.png)

![](https://cdn-images-1.medium.com/max/2000/1*sFrc5pZ3ibY73QwMvgfIgQ.png)

![](https://cdn-images-1.medium.com/max/2000/1*tNo-1MIw1QgqeckWS--fmw.png)

Order will save you.

Well, There are many ways of killing a rat!

[Let’s connect here](https://d-captainkenya.github.io/).

Happy Hacking.
