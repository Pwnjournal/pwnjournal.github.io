---
layout: post
title:  pwnable.kr rev mistake 
image: /assets/img/HTB-Logo.png
date:   2022-12-12 11:15:01
tags:
- rev
description: Basic rev challenge
categories:
- pwnable.kr
---

mistake pwnable.kr

mistake@pwnable:~$ ./mistake 
do not bruteforce...
0000000000
input password : 1111111111
Password OK
Mommy, the operator priority always confuses me :(

        int fd;
        if(fd=open("/home/mistake/password",O_RDONLY,0400) < 0){
                printf("can't open password %d\n", fd);
                return 0;
        }

        printf("do not bruteforce...\n");
        sleep(time(0)%20);

        char pw_buf[PW_LEN+1];
        int len;
        if(!(len=read(fd,pw_buf,PW_LEN) > 0)){
                printf("read error\n");
                close(fd);
                return 0;
        }

        char pw_buf2[PW_LEN+1];
        printf("input password : ");
        scanf("%10s", pw_buf2);

        // xor your input
        xor(pw_buf2, 10);

        if(!strncmp(pw_buf, pw_buf2, PW_LEN)){
                printf("Password OK\n");
                system("/bin/cat flag\n");
        }
        else{
                printf("Wrong Password\n");

You input the pass twice once xor and the second time without xor
The exe has a operator precendence problem when it reads fd the <0 executes before the = and then the descriptor will be 0 stdin