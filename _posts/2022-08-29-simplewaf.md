---
layout: post
title:  simplewaf
image: /assets/img/CORCTF 2022.PNG
date:   2022-08-29 21:52:19
tags:
- web
description: This is a request based challenge where you have to craft a get request such as it passes a filter for the flag keyword

categories:
- CORCTF 22
---

This is a request based challenge where you have to craft a get request such as it passes a filter for the flag keyword

Add `EXPOSE 3456` to the docker file so you can access the port on the docker.  
First of all in order to build the challenge you have to use the command `docker build .` it will give you an id at the end which you will use in order to run `docker run  
If you want to kill the docker you can list the docker images currently running look at the names it should be `charming_sammet` and get the id then using `docker kill id` you can kill it   
Also in order to make sure your docker will expose the port you should use the command `sudo docker container run -p 3456:3456 --expose 3456 2b420edb6b97` to run it.

The home page looks like:

![](/assets/img/2022-08-11-13-52-40.png)

Clicking on the hyperlink shows us the page

![](/assets/img/2022-08-11-13-57-26.png)

Making a double encoding script so we can better obfuscate the flag variable:
```
target_string='flag.txt'
def encode(target):
    new_target=''
    for i in target:
        new_target+='%'+format(ord(i), 'x')
    return new_target
encode(target_string).replace('%','%25')
```

TLDR:
![](/assets/img/2022-08-11-14-19-19.png)
The request that gets the flag is:

http://127.0.0.1:3456/?file[protocol]=file:&file[hostname]=&file[href]=None&file[origin]=None&file[pathname]=%2566%256c%2561%2567%252e%2574%2578%2574


How do we get here?

We need to evaluate the filtering function:

![](/assets/img/2022-08-11-14-21-41.png)

Also the function that fetches files:

![](/assets/img/2022-08-11-14-23-15.png)

The two functions we need to analyse are fs.readfilesync and JSON.stringify(item).includes.

Inputting different encodings

One encoding doesn't work because the json stringify decodes it. The double encoding option does not work as well because the req only does one unencode. Other options like utf stuff don't work as well.


The response on double encoding:


![](/assets/img/2022-08-11-15-08-21.png)

Looking at the function from github (https://github.com/nodejs/node/blob/main/lib/fs.js) we can see:




the function opensync opens the path we supply

![](/assets/img/2022-08-11-15-06-59.png)

Then the function openync uses getValidatedPath

![](/assets/img/2022-08-11-15-07-58.png)

The getvalidatedpath can retrieve a file or url:
![](/assets/img/2022-08-11-15-11-17.png)


And the function fileurl to path converts from our url to a path
![](/assets/img/2022-08-11-15-12-07.png)

And the function that gets a path from url also decodes again the input:

![](/assets/img/2022-08-11-15-14-54.png)

So we can bypass the filter by having double encoding if only we set the right parameters.


The query is :

http://127.0.0.1:3456/?file[protocol]=file:&file[hostname]=&file[href]=None&file[origin]=None&file[pathname]=%2566%256c%2561%2567%252e%2574%2578%2574


because we can see the following requirements:

`path.protocol=='file:'` fileurltopath function
`path.pathname=flag.txt encoded twice` getpathfromurlposix function 
`path.href not none` getvalidatedpath
`path.origin not none` getvalidatedpath
`path.hostname ==''` getpathfromurlposix function
