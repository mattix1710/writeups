---
title:
    - TrueSecrets
tags:
    - forensics
    - easy
---

!!! abstract "Description"
    Our cybercrime unit has been investigating a well-known APT group for several months. The group has been responsible for several high-profile attacks on corporate organizations. However, what is interesting about that case, is that they have developed a custom command & control server of their own. Fortunately, our unit was able to raid the home of the leader of the APT group and take a memory capture of his computer while it was still powered on. Analyze the capture to try to find the source code of the server.

For this CTF there was provided a zipped file with `.raw` extension. 

Examining the file with `strings` command revealed multiple informations about location on Windows system, e.g. `C:\Windows\system32\`.
This and information in the description indicates that this file is a memory capture of Windows system.

To process this file we can use Volatility framework 😊

At first we need to determine the system profile...
