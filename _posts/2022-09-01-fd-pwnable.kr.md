---
layout: post
title:  pwnable.kr rev fd pwnable 
image: /assets/img/HTB-Logo.png
date:   2022-12-12 11:15:01
tags:
- rev
description: Basic rev challenge
categories:
- pwnable.kr
---



```
fd@pwnable:~$ ls
fd  fd.c  flag
fd@pwnable:~$ cat fd.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
char buf[32];
int main(int argc, char* argv[], char* envp[]){
        if(argc<2){
                printf("pass argv[1] a number\n");
                return 0;
        }
        int fd = atoi( argv[1] ) - 0x1234;
        int len = 0;
        len = read(fd, buf, 32);
        if(!strcmp("LETMEWIN\n", buf)){
                printf("good job :)\n");
                system("/bin/cat flag");
                exit(0);
        }
        printf("learn about Linux file IO\n");
        return 0;

}

fd@pwnable:~$ ./fd
pass argv[1] a number
fd@pwnable:~$ ./fd 4661
LETMEWIN
good job :)
mommy! I think I know what a file descriptor is!!
```
An executable has 2 file descriptors by default stdin and stdout you need to pass an argument such that -0x1234 it is 1 corresponding to stdin such that when it reads from it it asks you for input.