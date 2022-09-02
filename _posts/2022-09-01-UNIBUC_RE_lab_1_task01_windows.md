---
layout: post
title:  UNIBUC_RE_lab_1_task01_windows
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:

description: The purpose is finding interesting stuff about a binary.

categories:

---

The purpose is finding interesting stuff about a binary.

The binary is not that obfuscated it uses xor to decrypt hardcoded strings.
![](/assets/img/2022-09-01-09-01-07.png)
Another one can be seen here:
![](/assets/img/2022-09-01-09-54-27.png)
By manually reversing the process we get:

![](/assets/img/2022-09-01-09-54-51.png)
Then the process calls create_mutex_a api so this part of the malware creates a mutex with the decoded name.

Next the malware wants to create a file using these strings:
![](/assets/img/2022-09-01-10-00-47.png)

The next decryptions are related to the registry:
SOFTWARE\Microsoft\Windows\CurrentVersion\Run
![](/assets/img/2022-09-01-10-04-23.png)
It sets the key WebLaunchAssist as a persistance mechanism

Furthermore it reads:
Software\Classes\http\shell\open\command

Then it tries to connect to `http://maybe.suspicious.to/secondstag` using a get request.

Using OSINT techniques we can see this is part of flare on challenge 12 (https://armerj.github.io/Flare-on-Challenge-12-Part-1/)