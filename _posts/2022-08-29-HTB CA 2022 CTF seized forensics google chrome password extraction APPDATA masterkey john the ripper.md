---
layout: post
title:  HTB CA 2022 CTF seized forensics google chrome password extraction APPDATA masterkey john the ripper
image: /assets/img/HTB CA 2022.jpg
date:   2022-08-29 21:52:19
tags:
- forensics
description: HTB CA 2022 CTF seized forensics google chrome password extraction APPDATA masterkey john the ripper

categories:
- CA HTB CTF
---

HTB CA 2022 CTF seized forensics google chrome password extraction APPDATA masterkey john the ripper

This CTF challenge deals with extracting the password from a google chorme LOGIN DATA database for the respective user.
The script says the following:
Miyuki is now after a newly formed ransomware division which works for Longhir. This division's goal is to target any critical infrastructure and cause financial losses to their opponents. They never restore the encrypted files, even if the victim pays the ransom. This case is the number one priority for the team at the moment. Miyuki has seized the hard-drive of one of the members and it is believed that inside of which there may be credentials for the Ransomware's Dashboard. Given the AppData folder, can you retrieve the wanted credentials? Download: http://134.209.177.115/forensics/forensics_seized.zip

My first try was indexing it with autopsy and seeing what vectors of recovering the password are present.
![](/assets/img/![](2022-05-16-09-39-41.png).png)
The next step is finding out what kind of encryption is present on the master key.
I found this helpful article https://nandynarwhals.org/sieberrsec-ctf-3.0-digginginthedump/ that i followed up untill he said that they already have the password.
To sum up:
1.You look inside the appdata folder for the chrome user data:
Python script:
import json, base64
x = json.load(open('AppData/Local/Google/Chrome/User Data/Local State', 'rb'))['os_crypt']['encrypted_key']
x = base64.b64decode(x)
open("blob",'wb').write(x[5:])

2.You look at the SID present in AppData/Roaming/Microsoft/Protect/ (the name of the folder S-* and the  file with random name inside). 

3.You take that file and run a script to convert it into a john hash:
I used this script https://github.com/openwall/john/blob/bleeding-jumbo/run/DPAPImk2john.py
and this command line:
    python3 Dpapi2.py --sid="S-1-5-21-3702016591-3723034727-1691771208-1002" --masterkey="865be7a6-863c-4d73-ac9f-233f8734089d" --context="local" > dpapi.dump

Then use john or your own tool to crack the hash:
 i used this command: john --wordlist=/usr/share/wordlists/rockyou.txt dpapi.dump 

Then go back to mimikatz because you have the password: and decrypt the master key
dpapi::masterkey /in:865be7a6-863c-4d73-ac9f-233f8734089d /sid:S-1-5-21-3702016591-3723034727-1691771208-1002 /password:ransom /protected

The script will give you the key:
[masterkey] with password: ransom (protected user)
  key : 138f089556f32b87e53c5337c47f5f34746162db7fe9ef47f13a92c74897bf67e890bcf9c6a1d1f4cc5454f13fcecc1f9f910afb8e2441d8d3dbc3997794c630
  sha1: a765463192eb14812650e62d6b0150a262d1864e

Use the script below to decrypt the blob we made in the first place
dpapi::blob /masterkey:138f089556f32b87e53c5337c47f5f34746162db7fe9ef47f13a92c74897bf67e890bcf9c6a1d1f4cc5454f13fcecc1f9f910afb8e2441d8d3dbc3997794c630 /in:"blob" /out:blob.dec

Using the blob we extracted there is a script on the above webpage, it didn't work for me so i used a modified one (same concept):
import os
import re
import sys
import json
import base64
import sqlite3
import win32crypt
from Crypto.Cipher import AES
import shutil
import csv
secret_key = open(r'K:\forensics_seized\blob.dec', 'rb').read()

chrome_path_login_db = r"K:\forensics_seized\Login Data"
conn = get_db_connection(chrome_path_login_db)
if(secret_key and conn):
    cursor = conn.cursor()
    cursor.execute("SELECT action_url, username_value, password_value FROM logins")
    for index,login in enumerate(cursor.fetchall()):
        url = login[0]
        username = login[1]
        ciphertext = login[2]
        initialisation_vector = ciphertext[3:15]
        encrypted_password = ciphertext[15:-16]
        cypher=AES.new(secret_key, AES.MODE_GCM, initialisation_vector)
        print(username,cypher.decrypt(encrypted_password))

The flag was there:
User Name: ransomoperator@draeglocker.com
Password: b'HTB{Br0ws3rs_C4nt_s4v3_y0u_n0w}'