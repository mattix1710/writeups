---
tags:
    - forensics
    - picoCTF2023
---

## Description
!!! abstract "Description"
    Someone just sent you an email claiming to be Google's co-founder Larry Page but you suspect a scam.
    Can you help us identify whose mail server the email actually originated from?
    Download the email file [here](https://artifacts.picoctf.net/c/363/email-export.eml). Flag: picoCTF{FirstnameLastname}

## Solve
Downloaded file contains a raw text of an e-mail with all the headers and additional data necessary to parse it and send.

From the `Received` section we can gather out the informations about IPv4 of the sender.

In the row 29 we can find such line:

```
Received: from mail.onionmail.org (mail.onionmail.org. [173.249.33.206])
```

Next we can use the `whois` tool that will backtrack the information about the sender.

Using command `whois 173.249.33.206` we can gather necessary informations and formulate the flag which is:

## Flag
``` title="flag"
picoCTF{WilhelmZwalina}
```