The Story

![an illustration depicting a wreath with ornaments](https://assets.tryhackme.com/additional/aoc2022/day11/banner.png)

Check out SecurityNinja's video walkthrough for Day 11 [here](https://www.youtube.com/watch?v=RsJR2z_agiY)!  

The elves in Santa's Security Operations Centre (SSOC) are hard at work checking their monitoring dashboards when Elf McDave, one of the workshop employees, knocks on the door. The elf says, _"I've just clicked on something and now my workstation is behaving in all kinds of weird ways. Can you take a look?"._

Elf McSkidy tasks you, Elf McBlue, to investigate the workstation. Running down to the workshop floor, you see a command prompt running some code. Uh oh! This is not good. You immediately create a memory dump of the workstation and place this dump onto your employee-issued USB stick, returning to the SSOC for further analysis.

_You plug the USB into your workstation and begin your investigation._

What is Memory Forensics?![a picture of elf mcblue](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/670492e08f9b74a3c38a5f6f219d2cdf.png)

Memory forensics is the analysis of the volatile memory that is in use when a computer is powered on. Computers use dedicated storage devices called Random Access Memory (RAM) to remember what is being performed on the computer at the time. RAM is extremely quick and is the preferred method of storing and accessing data. However, it is limited compared to storage devices such as hard drives. This type of data is volatile because it will be deleted when the computer is powered off. RAM stores data such as your clipboard or unsaved files. 

We can analyse a computer's memory to see what applications (processes), what network connections were being made, and many more useful pieces of information. For example, we can analyse the memory of a computer infected with malware to see what the malware was doing at the time.

Let's think about cooking. You normally store all of your food in the fridge - a hard drive is this fridge. When you are cooking, you will store ingredients on the kitchen counter so that you can quickly access them, but the kitchen counter (RAM) is much smaller than a fridge (hard drive)

Why is Memory Forensics Useful?![An image of a computer memory chip](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/50fa29c68caf89f0e3df5d19cfd40acd.png)

Memory forensics is an extremely important element when investigating a computer. A memory dump is a full capture of what was happening on the Computer at the time, for example, network connections or things running in the background. Most of the time, malicious code attempts to hide from the user. However, it cannot hide from memory.

We can use this capture of the memory for analysis at a later date, especially as the memory on the computer will eventually be lost (if, for example, we power off the computer to prevent malware from spreading). By analysing the memory, we can discover exactly what the malware was doing, who it was contacting, and such forth.

An Introduction to Processes

At the simplest, a process is a running program. For example, a process is created when running an instance of notepad. You can have multiple processes for an application (for example, running three instances of notepad will create three processes). This is important to know because being able to determine what processes were running on the computer will tell us what applications were running at the time of the capture.

On Windows, we can use Task Manager_(pictured below)_ to view and manage the processes running on the computer.

![A picture of Window's Task Manager](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/68217febd77dcd845ba608bb5ef6f34f.png)

_Window's Task Manager_

On a computer, processes are usually categorised into two groups:

**Category**

**Description**

**Example**

User Process

These processes are programs that the user has launched. For example, text editors, web browsers, etc.

notepad.exe - this is a text editor that is launched by the user.

Background Process

These processes are automatically launched and managed by the Operating System and are often essential to the Operating System behaving correctly.

dwm.exe - this is an essential process for Windows that is responsible for displaying windows and applications on the computer.

Introducing Volatility  

Volatility is an open-source memory forensics toolkit written in Python. Volatility allows us to analyse memory dumps taken from Windows, Linux and Mac OS devices and is an extremely popular tool in memory forensics. For example, Volatility allows us to:

-   List all processes that were running on the device at the time of the capture
-   List active and closed network connections
-   Use Yara rules to search for indicators of malware
-   Retrieve hashed passwords, clipboard contents, and contents of the command prompt
-   And much, much more!

Once Volatility and its requirements (i.e. Python) are installed, Volatility can be run using `python3 vol.py`. The terminal below displays Volatility's help menu:

Displaying Volatility's help menu

```shell-session
cmnatic@aoc2022-day-11:~/volatility3$ python3 vol.py -h
Volatility 3 Framework 2.4.1
usage: volatility [-h] [-c CONFIG] [--parallelism [{processes,threads,off}]] [-e EXTEND] [-p PLUGIN_DIRS] [-s SYMBOL_DIRS] [-v] [-l LOG] [-o OUTPUT_DIR] [-q]
                  [-r RENDERER] [-f FILE] [--write-config] [--save-config SAVE_CONFIG] [--clear-cache] [--cache-path CACHE_PATH] [--offline]
                  [--single-location SINGLE_LOCATION] [--stackers [STACKERS [STACKERS ...]]]
                  [--single-swap-locations [SINGLE_SWAP_LOCATIONS [SINGLE_SWAP_LOCATIONS ...]]]
                  plugin ...

An open-source memory forensics framework

optional arguments:
  -h, --help            Show this help message and exit, for specific plugin options use 'volatility  --help'
  -c CONFIG, --config CONFIG
  --cropped for brevity--
```

Today's task will cover Volatility 3, which was initially released in 2020 to replace the deprecated Volatility 2 framework.  Volatility requires  a few arguments to run:

-   Calling the Volatility tool via `python3 vol.py`
-   Any options such as the name and location of the memory dump
-   The action you want to perform (I.e. what plugins you want to use - we'll come onto these shortly!)

Some common options and examples that you may wish to provide to Volatility are located in the table below:

Option

Description

Example

-f

This argument is where you provide the name and location of the memory dump that you wish to analyse.

`python3 vol.py -f /path/to/my/memorydump.vmem`

-v

This argument increases the verbosity of Volatility. This is sometimes useful to understand what Volatility is doing in cases of debugging.

`python3 vol.py -v`   

-p

This argument allows you to override the default location of where plugins are stored.

`python3 vol.py -p /path/to/my/custom/plugins`  

-o

This argument allows you to specify where extracted processes or DLLs are stored.

`python3 vol.py -o /output/extracted/files/here`  

And finally, now we need to decide what we want to analyse the image for. Volatility uses plugins to perform analysis, such as:

-   Listing processes
-   Listing network connections
-   Listing contents of the clipboard, notepad, or command prompt
-   And much more! If you're curious, you can read the documentation [here](https://volatility3.readthedocs.io/en/latest/volatility3.plugins.html)

In this task, we are going to use Volatility to:

1.  See what Operating System the memory dump is from
2.  See what processes were running at the time of capture
3.  See what connections were being made at the time of capture

Using Volatility to Analyse an Image  

Before proceeding with our analysis, we need to confirm the Operating System of the device that the memory has been captured from. We need to confirm this because it will determine what plugins we can use in our investigation.

First, let's use the `imageinfo` plugin to analyse our memory dump file to determine the Operating System. To do this, we need to use the following command (remembering to include our memory dump by using the `-f` option): `python3 vol.py -f workstation.vmem windows.info`.

_Note: This can sometimes take a couple of minutes, depending on the size of the memory dump and the hardware of the system running the scan._

Using Volatility to gather some information about the memory dump

```shell-session
cmnatic@aoc2022-day-11:~/volatility3$ python3 vol.py -f workstation.vmem windows.info
Volatility 3 Framework 2.4.1
Progress:  100.00   PDB scanning finished
Variable  Value

Kernel Base 0xf803218a8000
DTB 0x1ad000
Symbols file:///home/ubuntu/volatility3/volatility3/symbols/windows/ntkrnlmp.pdb/E0093F3AEF15D58168B753C9488A4043-1.json.xz
Is64Bit True
IsPAE False
layer_name  0 WindowsIntel32e
memory_layer  1 FileLayer
KdVersionBlock  0xf80321cd23c8
Major/Minor 15.18362
MachineType 34404
KeNumberProcessors  4
SystemTime  2022-11-23 10:15:56
NtSystemRoot  C:\Windows
NtProductType NtProductWinNt
NtMajorVersion  10
NtMinorVersion  0
PE MajorOperatingSystemVersion  10
PE MinorOperatingSystemVersion  0
PE Machine  34404
PE TimeDateStamp  Mon Apr 14 21:36:50 2104
ubuntu@aoc2022-day-11:~/volatility3$
```

Great! We can see that Volatility has confirmed that the Operating System is Windows. With this information, we now know we need to use the Windows sub-set of plugins with Volatility. The plugins that are going to be used in today's task are detailed in the table below:

Plugin

Description

Objective

windows.pslist

This plugin lists all of the processes that were running at the time of the capture.

To discover what processes were running on the system.

windows.psscan

This plugin allows us to analyse a specific process further.

To discover what a specific process was actually doing.  

windows.dumpfiles

This plugin allows us to export the process, where we can perform further analysis (i.e. static or dynamic analysis).

To export a specific binary that allows us further to analyse it through static or dynamic analysis.  

windows.netstat

This plugin lists all network connections at the time of the capture.

To understand what connections were being made. For example, was a process causing the computer to connect to a malicious server? We can use this IP address to implement defensive measures on other devices. For example, if we know an IP address is malicious, and another device is communicating with it, then we know that device is also infected.

_Please note that this is not all of the possible plugins. An extensive list of the Windows sub-set of plugins can be found [here](https://volatility3.readthedocs.io/en/stable/volatility3.plugins.windows.html)._

  

Showing These Plugins in Use

windows.pslist

`python3 vol.py -f workstation.vmem windows.pslist`

Using windows.pslist

```shell-session
ubuntu@aoc2022-day-11:~/volatility3$ python3 vol.py -f workstation.vmem windows.pslist
Volatility 3 Framework 2.4.1
Progress:  100.00   PDB scanning finished
PID PPID  ImageFileName Offset(V) Threads Handles SessionId Wow64 CreateTime  ExitTime  File output

4 0 System  0xc0090b286040  141 - N/A False 2022-11-23 09:43:13.000000  N/A Disabled
104 4 Registry  0xc0090b2dd080  4 - N/A False 2022-11-23 09:43:04.000000  N/A Disabled
316 4 smss.exe  0xc0090e438400  2 - N/A False 2022-11-23 09:43:13.000000  N/A Disabled
436 428 csrss.exe 0xc0090ea65140  10  - 0 False 2022-11-23 09:43:18.000000  N/A Disabled
512 504 csrss.exe 0xc0090f35e140  12  - 1 False 2022-11-23 09:43:19.000000  N/A Disabled
536 428 wininit.exe 0xc0090f2c0080  1 - 0 False 2022-11-23 09:43:19.000000  N/A Disabled
584 504 winlogon.exe  0xc0090f383080  3 - 1 False 2022-11-23 09:43:19.000000  N/A Disabled
656 536 services.exe  0xc0090e532340  5 - 0 False 2022-11-23 09:43:20.000000  N/A Disabled
680 536 lsass.exe 0xc0090f3a5080  6 - 0 False 2022-11-23 09:43:20.000000  N/A Disabled
792 656 svchost.exe 0xc0090fa33240  12  - 0 False 2022-11-23 09:43:22.000000  N/A Disabled
820 536 fontdrvhost.ex  0xc0090f3a3140  5 - 0 False 2022-11-23 09:43:22.000000  N/A Disabled
828 584 fontdrvhost.ex  0xc0090fa39140  5 - 1 False 2022-11-23 09:43:22.000000  N/A Disabled
916 656 svchost.exe 0xc0090fad72c0  7 - 0 False 2022-11-23 09:43:23.000000  N/A Disabled
1000  584 dwm.exe 0xc0090fb0b080  13  - 1 False 2022-11-23 09:43:24.000000  N/A Disabled
380 656 svchost.exe 0xc0090fba9240  41  - 0 False 2022-11-23 09:43:25.000000  N/A Disabled
420 656 svchost.exe 0xc0090fbbf280  15  - 0 False 2022-11-23 09:43:25.000000  N/A Disabled
1116  656 svchost.exe 0xc0090fc2e2c0  16  - 0 False 2022-11-23 09:43:26.000000  N/A Disabled
1124  656 svchost.exe 0xc0090fc302c0  16  - 0 False 2022-11-23 09:43:26.000000  N/A Disabled
1204  656 svchost.exe 0xc0090fc2a080  19  - 0 False 2022-11-23 09:43:26.000000  N/A Disabled
1256  4 MemCompression  0xc0090fa35040  34  - N/A False 2022-11-23 09:43:26.000000  N/A Disabled
1292  656 svchost.exe 0xc0090fc752c0  2 - 0 False 2022-11-23 09:43:26.000000  N/A Disabled
1436  656 svchost.exe 0xc0090fdb52c0  7 - 0 False 2022-11-23 09:43:28.000000  N/A Disabled
--cropped for brevity--
```

  

windows.psscan

`python3 vol.py -f workstation.vmem windows.psscan`

Using windows.pscan

```shell-session
cmnatic@aoc2022-day-11:~/volatility3$ python3 vol.py -f workstation.vmem windows.psscan
Volatility 3 Framework 2.4.1
Progress:  100.00   PDB scanning finished
PID PPID  ImageFileName Offset(V) Threads Handles SessionId Wow64 CreateTime  ExitTime  File output

4 0 System  0xc0090b286040  141 - N/A False 2022-11-23 09:43:13.000000  N/A Disabled
104 4 Registry  0xc0090b2dd080  4 - N/A False 2022-11-23 09:43:04.000000  N/A Disabled
2528  2108  vm3dservice.ex  0xc0090b303080  2 - 1 False 2022-11-23 09:43:38.000000  N/A Disabled
2440  656 svchost.exe 0xc0090b336080  11  - 0 False 2022-11-23 09:43:37.000000  N/A Disabled
6584  792 ApplicationFra  0xc0090b375080  2 - 1 False 2022-11-23 09:58:58.000000  N/A Disabled
1048  656 SecurityHealth  0xc0090b39e080  9 - 0 False 2022-11-23 09:44:46.000000  N/A Disabled
1928  4064  cmd.exe 0xc0090b3a84c0  1 - 1 False 2022-11-23 09:59:09.000000  N/A Disabled
2040  5888  mysterygift.ex  0xc0090b52e4c0  3 - 1 False 2022-11-23 10:15:19.000000  N/A Disabled
316 4 smss.exe  0xc0090e438400  2 - N/A False 2022-11-23 09:43:13.000000  N/A Disabled
656 536 services.exe  0xc0090e532340  5 - 0 False 2022-11-23 09:43:20.000000  N/A Disabled
436 428 csrss.exe 0xc0090ea65140  10  - 0 False 2022-11-23 09:43:18.000000  N/A Disabled
536 428 wininit.exe 0xc0090f2c0080  1 - 0 False 2022-11-23 09:43:19.000000  N/A Disabled
512 504 csrss.exe 0xc0090f35e140  12  - 1 False 2022-11-23 09:43:19.000000  N/A Disabled
584 504 winlogon.exe  0xc0090f383080  3 - 1 False 2022-11-23 09:43:19.000000  N/A Disabled
820 536 fontdrvhost.ex  0xc0090f3a3140  5 - 0 False 2022-11-23 09:43:22.000000  N/A Disabled
680 536 lsass.exe 0xc0090f3a5080  6 - 0 False 2022-11-23 09:43:20.000000  N/A Disabled
792 656 svchost.exe 0xc0090fa33240  12  - 0 False 2022-11-23 09:43:22.000000  N/A Disabled
1256  4 MemCompression  0xc0090fa35040  34  - N/A False 2022-11-23 09:43:26.000000  N/A Disabled
828 584 fontdrvhost.ex  0xc0090fa39140  5 - 1 False 2022-11-23 09:43:22.000000  N/A Disabled
916 656 svchost.exe 0xc0090fad72c0  7 - 0 False 2022-11-23 09:43:23.000000  N/A Disabled
--cropped for brevity--
```

  

windows.dumpfiles

`python3 vol.py -f workstation.vmem windows.dumpfiles`

Using windows.dumpfiles

```shell-session
cmnatic@aoc2022-day-11:~/volatility3$ python3 vol.py -f workstation.vmem windows.dumpfiles --pid 4640
Volatility 3 Framework 2.4.1
Progress:  100.00   PDB scanning finished
Cache FileObject  FileName  Result

ImageSectionObject  0xc00910256a80  WinStore.App.exe  dumping file
DataSectionObject 0xc0090fc4bae0  ~FontCache-FontFace.dat dumping file
ImageSectionObject  0xc00911d3f740  Windows.UI.Xaml.winmd dumping file
DataSectionObject 0xc00910d27210  ~FontCache-S-1-5-21-4089795901-3714076801-2393801563-1000.dat dumping file
ImageSectionObject  0xc0091491f6a0  Windows.UI.Xaml.Resources.rs5.dll dumping file
ImageSectionObject  0xc0091123add0  Windows.ApplicationModel.winmd  dumping file
--cropped for brevity--
```

![A picture of the Bandit Yeti APT group logo](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/0365a56aba829ffc28205e4f3db0fbc2.png)

To access the memory dump, you will need to deploy the machine attached to this task by pressing the green "Start Machine" button located at the top-right of this task. The machine should launch in a split-screen view. If it does not, you will need to press the blue "Show Split Screen" button near the top-right of this page.   

**_Volatility and the memory file (named workstation.vmem) is located in /home/elfmcblue/volatility3._**