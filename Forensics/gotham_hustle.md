# Gotham Hustle

## Description
Description: Gotham’s underbelly trembles as whispers spread—The Riddler’s back, leaving cryptic puzzles across the city’s darkest corners. Every clue is a trap, every answer another step into madness. Think you can outsmart him? Step into Gotham’s shadows and prove it. Let the Batman's Hustle get its recognition!

Author: Azr43lKn1ght

## Solve
**Flag:** `bi0sctf{w3lc0m3_t0_df1r_l4b5_h0p3_th15_b3n3f175_y0u_m0r3_13337431}`

- In this challenge, a .raw file is given which we can analyze using volatility tool. It is important to take into consideration the version of volatility tool as this challenge can only be solved using volatility 2 and not by volatility 3. Initially I tried to solve this using volatility 3 as I didn't know about the constraint of this challenge but when none of the commands gave a proper output except `windows.psscan` and `windows.netscan`, I had to download the volatility 2 standalone version.
- On the volatility 2, the first thing was to check the imageinfo of the gotham.raw file: 

```
PS C:\Users\adity\Downloads\forensics> .\vol.exe -f gotham.raw imageinfo
Volatility Foundation Volatility Framework 2.6
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_23418
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (C:\Users\adity\Downloads\forensics\gotham.raw)
                      PAE type : No PAE
                           DTB : 0x187000L
                          KDBG : 0xf80002a48120L
          Number of Processors : 6
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0xfffff80002a4a000L
                KPCR for CPU 1 : 0xfffff880009ea000L
                KPCR for CPU 2 : 0xfffff88002ea8000L
                KPCR for CPU 3 : 0xfffff88002f24000L
                KPCR for CPU 4 : 0xfffff88002fa0000L
                KPCR for CPU 5 : 0xfffff88002fdc000L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2024-08-06 18:37:19 UTC+0000
     Image local date and time : 2024-08-06 11:37:19 -0700
```

-  I listed all the processes using pslist as below:

```
PS C:\Users\adity\Downloads\forensics> .\vol.exe -f gotham.raw --profile=Win7SP1x64 pslist
Volatility Foundation Volatility Framework 2.6
Offset(V)          Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit
------------------ -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------
0xfffffa80036d4040 System                    4      0    113      573 ------      0 2024-08-07 04:19:36 UTC+0000
0xfffffa8004969040 smss.exe                312      4      2       34 ------      0 2024-08-07 04:19:36 UTC+0000
0xfffffa80064e2b00 csrss.exe               400    388      9      524      0      0 2024-08-07 04:19:42 UTC+0000
0xfffffa8006433360 wininit.exe             452    388      3       81      0      0 2024-08-07 04:19:42 UTC+0000
0xfffffa80063c7b00 csrss.exe               460    444     12      535      1      0 2024-08-07 04:19:42 UTC+0000
0xfffffa8006685280 winlogon.exe            516    444      5      123      1      0 2024-08-07 04:19:43 UTC+0000
0xfffffa8006682b00 services.exe            552    452      7      228      0      0 2024-08-07 04:19:43 UTC+0000
0xfffffa80066d7060 lsass.exe               568    452      9      745      0      0 2024-08-07 04:19:44 UTC+0000
0xfffffa80066d5b00 lsm.exe                 576    452     10      154      0      0 2024-08-07 04:19:44 UTC+0000
0xfffffa8006714b00 svchost.exe             680    552     11      375      0      0 2024-08-07 04:19:45 UTC+0000
0xfffffa80048eb8e0 VBoxService.ex          744    552     13      124      0      0 2024-08-07 04:19:45 UTC+0000
0xfffffa80064de5c0 svchost.exe             804    552      8      305      0      0 2024-08-06 15:49:46 UTC+0000
0xfffffa80065c8750 svchost.exe             896    552     23      593      0      0 2024-08-06 15:49:46 UTC+0000
0xfffffa80067c4860 svchost.exe             936    552     27      610      0      0 2024-08-06 15:49:46 UTC+0000
0xfffffa80067d0b00 svchost.exe             968    552     20      651      0      0 2024-08-06 15:49:46 UTC+0000
0xfffffa80067e43e0 svchost.exe            1004    552     36     1041      0      0 2024-08-06 15:49:46 UTC+0000
0xfffffa8006816b00 svchost.exe            1064    552     18      494      0      0 2024-08-06 15:49:47 UTC+0000
0xfffffa8006905590 spoolsv.exe            1280    552     14      323      0      0 2024-08-06 15:49:48 UTC+0000
0xfffffa800691a280 svchost.exe            1308    552     16      320      0      0 2024-08-06 15:49:48 UTC+0000
0xfffffa800688cb00 svchost.exe            1400    552     10      156      0      0 2024-08-06 15:49:49 UTC+0000
0xfffffa80069a8b00 svchost.exe            1428    552     20      366      0      0 2024-08-06 15:49:50 UTC+0000
0xfffffa80038c4b00 dwm.exe                1556    936      3      110      1      0 2024-08-06 15:49:58 UTC+0000
0xfffffa8006bbeb00 explorer.exe           1172   2024     32     1017      1      0 2024-08-06 15:49:58 UTC+0000
0xfffffa80067b0b00 taskhost.exe           1328    552      8      216      1      0 2024-08-06 15:49:58 UTC+0000
0xfffffa8006c5f780 VBoxTray.exe           2240   1172     17      165      1      0 2024-08-06 15:50:00 UTC+0000
0xfffffa8006bc0b00 SearchIndexer.         2460    552     13      664      0      0 2024-08-06 15:50:06 UTC+0000
0xfffffa80038ce060 wmpnetwk.exe           2784    552     33      462      0      0 2024-08-06 15:50:32 UTC+0000
0xfffffa8006d90b00 svchost.exe            2888    552     10      372      0      0 2024-08-06 15:50:33 UTC+0000
0xfffffa8004887410 sppsvc.exe              912    552      7      157      0      0 2024-08-06 15:52:00 UTC+0000
0xfffffa800673bb00 svchost.exe            1080    552     13      385      0      0 2024-08-06 15:52:01 UTC+0000
0xfffffa8004587060 GoogleCrashHan         3044   4908      5       92      0      1 2024-08-06 16:36:44 UTC+0000
0xfffffa80039c5b00 GoogleCrashHan          408   4908      5       85      0      0 2024-08-06 16:36:44 UTC+0000
0xfffffa80044c3b00 chrome.exe             4456   4464     32     1296      1      0 2024-08-06 16:36:45 UTC+0000
0xfffffa8004463060 chrome.exe             4432   4456      8      119      1      0 2024-08-06 16:36:45 UTC+0000
0xfffffa8003daeb00 chrome.exe             4928   4456     13      234      1      0 2024-08-06 16:36:47 UTC+0000
0xfffffa8004511b00 chrome.exe             4872   4456      8      155      1      0 2024-08-06 16:36:47 UTC+0000
0xfffffa8003ff9060 chrome.exe             4612   4456     17      229      1      0 2024-08-06 16:36:55 UTC+0000
0xfffffa8004846060 taskhost.exe           3620    552      5       96      1      0 2024-08-06 16:37:06 UTC+0000
0xfffffa8004412060 chrome.exe             4204   4456     11      191      1      0 2024-08-06 16:37:16 UTC+0000
0xfffffa8004403b00 cmd.exe                3944   1172      1       20      1      0 2024-08-06 16:45:56 UTC+0000
0xfffffa8006d58060 conhost.exe            4188    460      2       53      1      0 2024-08-06 16:45:56 UTC+0000
0xfffffa8003c9c4f0 notepad.exe            2592   1172      1       58      1      0 2024-08-06 16:47:20 UTC+0000
0xfffffa8003f3cb00 chrome.exe             3764   4456     17      253      1      0 2024-08-06 17:24:44 UTC+0000
0xfffffa8003cebb00 chrome.exe             2608   4456     17      250      1      0 2024-08-06 17:24:45 UTC+0000
0xfffffa800447c060 chrome.exe             3612   4456     17      253      1      0 2024-08-06 17:24:48 UTC+0000
0xfffffa800446a9a0 chrome.exe             3172   4456     17      258      1      0 2024-08-06 17:24:52 UTC+0000
0xfffffa800443e060 chrome.exe             3704   4456     17      253      1      0 2024-08-06 17:24:55 UTC+0000
0xfffffa80047c5060 chrome.exe             4452   4456     17      270      1      0 2024-08-06 17:25:33 UTC+0000
0xfffffa8004031290 chrome.exe             4836   4456     17      241      1      0 2024-08-06 17:26:01 UTC+0000
0xfffffa8003d47860 chrome.exe             2168   4456     17      231      1      0 2024-08-06 17:27:52 UTC+0000
0xfffffa8004a7fb00 chrome.exe             3808   4456     21      254      1      0 2024-08-06 18:15:29 UTC+0000
0xfffffa800437b4d0 chrome.exe             3740   4456     12      164      1      0 2024-08-06 18:32:47 UTC+0000
0xfffffa8003e86060 taskeng.exe            4196   1004      4       89      0      0 2024-08-06 18:33:18 UTC+0000
0xfffffa80039c2490 mspaint.exe            2516   1172      7      142      1      0 2024-08-06 18:35:09 UTC+0000
0xfffffa8003d94060 svchost.exe            1648    552      7      112      0      0 2024-08-06 18:35:09 UTC+0000
0xfffffa80044d6700 SearchProtocol         4436   2460      9      285      0      0 2024-08-06 18:36:43 UTC+0000
0xfffffa80040403e0 SearchFilterHo         1496   2460      6      105      0      0 2024-08-06 18:36:43 UTC+0000
0xfffffa800491c600 audiodg.exe            4028    896      6      132      0      0 2024-08-06 18:37:14 UTC+0000
0xfffffa8003bcf420 DumpItog.exe           4960   1172      5       56      1      1 2024-08-06 18:37:17 UTC+0000
0xfffffa80045dca30 conhost.exe            4140    460      2       53      1      0 2024-08-06 18:37:17 UTC+0000
```
- Here, we can clearly see there are multiple sub-processes that begin from a single process but in all this, there are 5 processes that stand out. They are `cmd.exe`, `notepad.exe`, `mspaint.exe`, `DumpItog.exe` and `VBoxTray.exe`. They all begin with the PPID 1172 which is the `explorer.exe`.
- So my first clue was to try and search through these processes in order to find the flag.
- Since one of the processes was `cmd.exe` I tried the following command to check the information in it:

```
PS C:\Users\adity\Downloads\forensics> .\vol.exe -f gotham.raw --profile=Win7SP1x64 cmdscan
Volatility Foundation Volatility Framework 2.6
**************************************************
CommandProcess: conhost.exe Pid: 4188
CommandHistory: 0x130de0 Application: cmd.exe Flags: Allocated, Reset
CommandCount: 8 LastAdded: 7 LastDisplayed: 7
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x60
Cmd #0 @ 0x12f7c0: whoami
Cmd #1 @ 0x130110: dir
Cmd #2 @ 0x12f800: bi0s
Cmd #3 @ 0x12f820: dfirlabs
Cmd #4 @ 0x12d690: Ymkwc2N0Znt3M2xjMG0zXw==
Cmd #5 @ 0x12d6d0: azr43ln1ght.github.io
Cmd #6 @ 0x125650: Azr43lKn1ght
Cmd #7 @ 0x125680: did you find flag1?
Cmd #15 @ 0xb0158:
Cmd #16 @ 0x12ff50:
**************************************************
CommandProcess: conhost.exe Pid: 4140
CommandHistory: 0x280e10 Application: DumpItog.exe Flags: Allocated
CommandCount: 0 LastAdded: -1 LastDisplayed: -1
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x10
Cmd #15 @ 0x200158: (
Cmd #16 @ 0x27ff70: (
```

- Here, I clearly got a base64 ciphered text: 

> ciphered text: Ymkwc2N0Znt3M2xjMG0zXw==
> deciphered text: bi0sctf{w3lc0m3_

- Now I got the first part of the flag out of many and I know that all the flag files will be labelled as flag[number]. So that was a big clue to finding the other flags.
- Now, I tried checking the files in gotham.raw and filtered them for the word flag. There, I saw the flag5.rar file. Knowing that it was an archive that can be opened using WinRAR, I dumped the memory of the file using its offset and then opened it. 

```
PS C:\Users\adity\Downloads\forensics> .\vol.exe -f gotham.raw --profile=Win7SP1x64 filescan | select-string "flag"
Volatility Foundation Volatility Framework 2.6

0x000000011fdaff20     16      0 -W-r-- \Device\HarddiskVolume2\Users\bruce\Desktop\flag5.rarp\VirtualBox Dropped
Files\2024-08-06T18_36_43.522668500Z\flag5.rar
PS C:\Users\adity\Downloads\forensics> .\vol.exe -f gotham.raw --profile=Win7SP1x64 dumpfiles -Q 0x000000011fdaff20 -D .
Volatility Foundation Volatility Framework 2.6
DataSectionObject 0x11fdaff20   None   \Device\HarddiskVolume2\Users\bruce\Desktop\flag5.rarp\VirtualBox Dropped Files\2024-08-06T18_36_43.522668500Z\flag5.rar
```

![alt text](<Screenshot 2025-12-19 093207.png>)

- From the image, we get the clue that **The password for the zip file is the computer's password**. Now, since we are working on the raw image of Bruce's computer I needed the password for the user Bruce. For that, I ran the hashdump command to find all the passwords and then decoded Bruce's password which turned out to be `batman`.

``` 
PS C:\Users\adity\Downloads\forensics> .\vol.exe -f gotham.raw --profile=Win7SP1x64 hashdump
Volatility Foundation Volatility Framework 2.6
Administrator:500:aad3b435b51404eeaad3b435b51404ee:10eca58175d4228ece151e287086e824:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
bruce:1001:aad3b435b51404eeaad3b435b51404ee:b7265f8cc4f00b58f413076ead262720:::
HomeGroupUser$:1002:aad3b435b51404eeaad3b435b51404ee:bda4ed0acc67d6d60540d1a20cf444c6:::
```
- From the flag5.rar, I got the 5th part of the flag:

> ciphered text in flag5.rar: bTByM18xMzMzNzQzMX0= 
> deciphered text: m0r3_13337431}

- Now I had 2 parts of the flag. Next, I tried the `notepad.exe` process. I assumed that since it is notepad, the flag might have been written somewhere in its memory. So first I dumped the memory of this process and then searched for flag2, flag3, flag4 through it:

```
PS C:\Users\adity\Downloads\forensics> .\vol.exe -f gotham.raw --profile=Win7SP1x64 memdump -p 2592 -D .
Volatility Foundation Volatility Framework 2.6
************************************************************************
Writing notepad.exe [  2592] to 2592.dmp
PS C:\Users\adity\Downloads\forensics> strings -e l 2592.dmp | select-string "flag2"
PS C:\Users\adity\Downloads\forensics> strings -e l 2592.dmp | select-string "flag3"

flag3 = aDBwM190aDE1Xw== - Google Search - Google Chrome
flag3 = aDBwM190aDE1Xw== - Google Search - Google Chrome
flag3 = aDBwM190aDE1Xw== - Google Search
https://www.google.com/search?q=flag3+%3D+aDBwM190aDE1Xw%3D%3D&oq=flag3+%3D+aDBwM190aDE1Xw%3D%3D&aqs=
chrome..69i57j0i512i546l2.321545j0j7&sourceid=chrome&ie=UTF-8
https://www.google.com/search?q=flag3+%3D+aDBwM190aDE1Xw%3D%3D&oq=flag3+%3D+aDBwM190aDE1Xw%3D%3D&aqs=
chrome..69i57j0i512i546l2.321545j0j7&sourceid=chrome&ie=UTF-8
https://www.google.com/search?q=flag3+%3D+aDBwM190aDE1Xw%3D%3D&oq=flag3+%3D+aDBwM190aDE1Xw%3D%3D&aqs=
chrome..69i57j0i512i546l2.321545j0j7&sourceid=chrome&ie=UTF-8
https://www.google.com/search?q=flag3+%3D+aDBwM190aDE1Xw%3D%3D&oq=flag3+%3D+aDBwM190aDE1Xw%3D%3D&aqs=
chrome..69i57j0i512i546l2.321545j0j7&sourceid=chrome&ie=UTF-8


PS C:\Users\adity\Downloads\forensics> strings -e l 2592.dmp | select-string "flag4"

flag4 = YjNuM2YxNzVfeTB1Xw==
```

- Fortunately, I was able to retrieve flag3 and flag4 from this process:

> flag 3 cipher text: aDBwM190aDE1Xw==
> flag 3 deciphered text: _h0p3_th15_
> flag 4 cipher text: YjNuM2YxNzVfeTB1Xw==
> flag 4 deciphered text: b3n3f175_y0u_

- Now, the only part that remained was flag2 and I still had 3 processes where it could potentially be present. I started with `mspaint.exe`. Since it is a paint process, I was skeptical whether the flag could be searched through in its memory like I did with notepad. But I still dumped its memory and ran a search for flag2 through it but nothing turned up. Now I was sure that the flag is probably an image. 
- Since I had already dumped the memory, I changed the extension of the memory dump from .dmp to .data and opened it using GIMP. Using that application, I had to experiment with the width and height until the some of the text eventually aligned properly. This flag decode to : 
> t0_df1r_l4b5_

- Now I had my full flag as I already wrote above.