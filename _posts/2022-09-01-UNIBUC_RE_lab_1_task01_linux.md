---
layout: post
title:  UNIBUC_RE_lab_1_task01_linux
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- rev
description: This task contains a file that was striped, you need to open it and give the good password
categories:
- UNIBUC RE
---

This task contains a file that was striped, you need to open it and give the good password

Linux disassembly with
objdump -M intel -D ./crackme  >disass 
Windows IDA we look at the strings and see :
![](/assets/img/2022-08-31-19-14-35.png)

After going to the reference we discover that there is a huge blob of text not recognized to be a function.

I was curious as to what was causing the sig_segv and i saw two sigaction calls:
![](/assets/img/2022-09-01-08-19-55.png)

Looking at the signal numbers:
![](/assets/img/2022-09-01-08-21-59.png)
We can see that one of them was for sigsegv and another for sigkill. That means this signals are planed.

Because this is a blackbox challenge, we need to examine it another way.

We need to see if something changes from more input. We can see that it does with the following script:


```
#!/usr/bin/python

from pwn import * 

iteration=1
while(True):
	p=process('ltrace ./crackme', shell=True)
	process_output=p.recvuntil(b'fgets')
	p.sendline(iteration*b'A')	
	process_output=p.recvuntil(b'WRO')	
	iteration+=1
	if(len(process_output.split(b'\n'))>5):
		print("-XXXXXXXXXXXXX-")
		print(process_output)
		print(iteration)	
		break

	p.close()	
```
We can see that it uses strstr to find a wierd string.
![](/assets/img/2022-09-01-08-31-28.png)

By putting the strings in the output we see more and more strings until we get them all


```
from pwn import *
import copy
import pwn
def backtrack(sols):
	solution_strings=['nhnewfhetk','mdcdyamgec','zihldazjcn','vlrgmhasbw','jqvanafylz','hhqtjylumf','yemlopqosj'] 
	if len(sols)==len(solution_strings):
		print('-------------------Begining-----------------------')
		print(sols)
		p=process('ltrace ./crackme', shell=True)
		p.recvuntil(b'fgets')
		p.send(''.join(sols)+'\n')

		all_outputs=[]
		while(1):
			
			proc_output=p.recvline()
			all_outputs.append(proc_output)
			if(b'putchar(10,' in proc_output):
				try:
						data = proc_output.decode("utf-8")
						print(ord(data[0]))
						print(len(data))
						print(data)
						if(len(data)>0):
						 print("------------Here-------------------")
						 print("------------Here-------------------")
						 exit()
				except (UnicodeDecodeError, AttributeError):
						pass
				print(proc_output)
				break
			if(b'exited' in proc_output):
				print(call_outputs)
				break
			
		print('--------------------End----------------------')
		p.kill()
		return

	back=copy.deepcopy(sols)
	for i in range(len(solution_strings)):
		if  solution_strings[i] not in sols:
			back.append(solution_strings[i])
			backtrack(back)
			
			back=back[:-1]

			
backtrack([])	
```
timctf{7dfadd1ee67a9c516c9efbf8f0cf43f4}

Now in order to trace this to its original roots we search the flag online and find that it was in a 2018 ctf from timisoara. We see that the person who solved it used (https://kirschju.re/demov) and that the binary was obfuscated using https://github.com/xoreaxeaxeax/movfuscator