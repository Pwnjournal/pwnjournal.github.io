---
layout: post
title:  Wide CA 2022 Reverse
image: /assets/img/HTB CA 2022.jpg
date:   2022-08-29 21:52:19
tags:
- rev
description: Wide CA 2022 Reverse

categories:
- CA HTB CTF
---

Wide CA 2022 Reverse
![](/assets/img/2022-05-19-14-26-12.png)
We've received reports that Draeger has stashed a huge arsenal in the pocket dimension Flaggle Alpha. You've managed to smuggle a discarded access terminal to the Widely Inflated Dimension Editor from his headquarters, but the entry for the dimension has been encrypted. Can you make it inside and take control?

This reverse is easy, opening the file with the Ida debugger we can see that the app has a menu,
![](/assets/img/2022-05-19-15-24-38.png)
By examining the menu we see that the last selection is encrypted, and it asks us a password.
Navigating to that part in ida we see that the read password is compared with something that begins with an "s". We discover that the string is misrepresented by ida and it is in fact another string. After converting the string (CTRL-A to open the menu and select whatever has 32 bits)
![](/assets/img/2022-05-19-15-26-35.png)
Then enter the password in the prompt either wide or normal chars and enjoy the flag
![](/assets/img/2022-05-19-15-28-54.png)
HTB{str1ngs_4r3nt_4lw4ys_4sc11}