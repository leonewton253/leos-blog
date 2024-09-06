---
author: Leo Newton
pubDatetime: 2024-09-05T12:55:00Z
modDatetime: 
title: Changing Lenovo Thinkpad X13S IMEI
slug: thinkpad-imei-change
featured: true
draft: false
tags:
  - hacking
  - esim
  - imei
description:
  Hacking the Thinkpad x13s to allow using mobile phone SIMs with a laptop GSM modem
---

<img src="https://i.pcmag.com/imagery/reviews/008qHtyerRycV1ifD1a1qMl-1.fit_scale.size_1028x578.v1660865414.jpg" alt="picture of thinkpad x13s">




### Download Special Ubuntu Image with working kernel parameters and DTB table

https://cdimage.ubuntu.com/daily-live/current/

https://cdimage.ubuntu.com/daily-live/current/oracular-desktop-arm64.iso

### Boot Live image

Boot live image. Follow directions here: https://discourse.ubuntu.com/t/status-of-ubuntu-support-for-lenovo-thinkpad-x13s/44652


### Mask and stop the services:
``sudo systemcti disable NetworkManager ModemManager``

``sudo systemctl stop NetworkManager ModemManager``
### Manually run with debug enabled:
``sudo /us/sbin/ModemManager -debug``
### Open a new terminal, find the modem index and send the AT command:
``sudo mmeli-m -command="AT``
(mmcli -L to find the index)

### IMEI Repair
You can also change the IMEI AT command, for this IMEl must be converted to GSM format.
1. As an example, let's assume we want to set the IMEl to
123456789012347
2. To the left of the IMEl add 80A:
80A123456789012347
3. Break the numbers into pairs:
80,A1,23,45,67,89,01,23,47
4. Reverse the two numbers in each pair:
08,1A,32,54,76,98,10,32,74
Remove the SIM card
6. Clear the old IMEl using this AT command:
AT^NV=550,0
7. Confirm AT-GETIMEl now shows
+CME ERROR: memory failure
Set the IMEl, inserting the number calculated above:
AT^NV=550,9,"08,1A,32,54,76,98,10,32,74"
9. Confirm the IMEI was changed:
AT GETIMEI should show the new IMEI

<img src="/leonewton253/leos-blog/src/assets/images/lenovo-terminal-imei.jpg" alt="terminal with imei changed">


## Sources
https://techship.com/support/faq/how-to-send-at-commands-through-modem-manager/

https://docs.google.com/document/d/1kPl4pIGCkxKC07l9P7AnT7IfYt36lia5Fxoeh-tdhw4/edit?pli=1#heading=h.nj1rjoo4g5vx