---
title: TryHackMe Industrial Intrusion CTF 2025
author: D_C4ptain
date: 2025-05-02 10:15:00 +0300
categories: [CTF, write up]
tags: [TryHackMe, Industrial Intrusion ctf, rev, reversing, reverse engineering, D_c4ptain, jeopardy, Capture The Flag, ctf, fr334aks-mini, fr334aks, write up]
---

## Introduction

![t1.png](/assets/img/posts/ctf/thmindustrialintrusionctf25/t1.png)

On Fri, 27th June 2025 — Sun, 29th June 2025, We [Fr334aks-mini](https://www.linkedin.com/company/83010158/) had some fun playing the Industrial Intrusion CTF by TryHackMe.

It hosted challenges in:

- OSINT
- Web 3
- Networking
- Forensics
- Boot2Root
- Cryptography
- SCADA/ICS
- ...And more!

I will break down one of the challenges I solved.


## Reverse Engineering

### Auth

We re given a zip file and a remote server.

Checking the unzipped file type, we see it's a non-stripped ELF. From the strings it seems we need to enter some code te get the flag.

![a1.png](/assets/img/posts/ctf/thmindustrialintrusionctf25/a1.png)

On running it, I tried a couple of "codes" and got denials. I opened Ghidra to see what was fueling the denial fire.

![a2.png](/assets/img/posts/ctf/thmindustrialintrusionctf25/a2.png)


We've got 2 functions of interest; main and transform.

![a3.png](/assets/img/posts/ctf/thmindustrialintrusionctf25/a3.png)

The main function:
``` c

undefined8 main(void)

{
  int iVar1;
  char *pcVar2;
  undefined8 uVar3;
  size_t sVar4;
  FILE *__stream;
  long in_FS_OFFSET;
  undefined8 local_168;
  undefined8 local_160;
  undefined8 local_158 [8];
  char local_118 [264];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  local_160 = 0xefcdab8967452301;
  printf("[?] Enter unlock code: ");
  pcVar2 = fgets((char *)local_158,0x40,stdin);
  if (pcVar2 == (char *)0x0) {
    fwrite("Error reading input\n",1,0x14,stderr);
    uVar3 = 1;
  }
  else {
    sVar4 = strcspn((char *)local_158,"\r\n");
    *(undefined1 *)((long)local_158 + sVar4) = 0;
    sVar4 = strnlen((char *)local_158,0x40);
    if (sVar4 == 8) {
      local_168 = local_158[0];
      transform(&local_168,8);
      iVar1 = memcmp(&local_168,&local_160,8);
      if (iVar1 == 0) {
        __stream = fopen("flag.txt","r");
        if (__stream == (FILE *)0x0) {
          perror("fopen");
          uVar3 = 1;
        }
        else {
          pcVar2 = fgets(local_118,0x100,__stream);
          if (pcVar2 == (char *)0x0) {
            fwrite("Error reading flag\n",1,0x13,stderr);
          }
          else {
            printf("[+] Access Granted! Flag: %s",local_118);
          }
          fclose(__stream);
          uVar3 = 0;
        }
      }
      else {
        puts("[!] Access Denied!");
        uVar3 = 1;
      }
    }
    else {
      puts("[!] Access Denied!");
      uVar3 = 1;
    }
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return uVar3;
}

```

The transform function:
```c

void transform(long param_1,ulong param_2)

{
  undefined8 local_10;
  
  for (local_10 = 0; local_10 < param_2; local_10 = local_10 + 1) {
    *(byte *)(local_10 + param_1) = *(byte *)(local_10 + param_1) ^ 0x55;
  }
  return;
}

```


+ [x] Understanding the binary

Main function:

It reads an 8-byte code and checks if the input length is exactly 8 bytes.
Applies transform to the input and compares the transformed input with a hardcoded value at (0xefcdab8967452301). If they match, it prints the flag from flag.txt.

Transform function:

It takes a buffer (param_1) and its length (param_2) then XORs each byte in the buffer with 0x55.


+ [x] Reverse the unlock code

The binary compares the transformed input with 0xefcdab8967452301.

Due to little-endian storage(which I struggled with), this is stored in memory as: ```01 23 45 67 89 ab cd ef```

To find the correct input, we XOR each target byte with 0x55 to get ```\x54\x76\x10\x32\xdc\xfe\x98\xba```

+ [x] Targeting the remote server

```
echo -e "\x54\x76\x10\x32\xdc\xfe\x98\xba" | nc 10.10.255.232 9005
```

`THM{Simple_tostart_nice_done_mwww}`


+ [x] Conclusion

We just needed to XOR each byte of 0xefcdab8967452301 (little-endian) with 0x55 to get \x54\x76\x10\x32\xdc\xfe\x98\xba and send this to the server.


Till the next one.

And remember, there are many ways of killing Jerry!

![jerry.webp](/assets/img/jerry.webp)