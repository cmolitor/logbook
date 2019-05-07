---
layout:     post
title:      How to setup the Adeunis Field Test Device under macOS
author:     Christoph Molitor
tags:       LoRa LoRaWAN macOS cheatsheet
subtitle:   Configuring LoRaWAN Adeunis Field Test device using macOS and CoolTerm
category:   report
---
<!-- Start Writing Below in Markdown -->


## Background

Lately I was testing the LoRaWAN coverage of our deployed gateways with an [Adeunis Field Test Device](https://www.adeunis.com/en/produit/ftd-868-915-2/). 
For the coverage test I wanted to change some settings of Adeunis Field Test Device. More specific: I wanted the device to send an Uplink message more frequently (default interval is 10 minutes).

Looking for a manual to change the settings of the device, I found the following detailed device description (german) provided by 
[Smartmakers](https://smartmakers.io/de/test-adeunis-868-field-test-device/).

However, it took me a while to figure out how to configure the Adeunis Field Test Device using macOS and CoolTerm, as the Smartmaker description is for Windows systems. So here a short description how to do it on a Mac. Check out also the video demo below.

## General setup

As I am using a Mac, I found and used [CoolTerm](https://freeware.the-meiers.org) to connect via a serial connection with the Adeunis.
I connected the Adeunis Field Test Device with an USB cable with my Mac. 

## Steps to change settings of Adeunis Field Test Device

Here a short description of the steps to change the settings of the Adeunis Field Test Device. Please see the video for details.

1. Connecting to Adeunis Field Test Device

    1. Connect Adeunis Field Test Device via USB cable
    1. Turn Adeunis Field Test Device on
    1. Open CoolTerm
    1. Connection (menu) -> Options -> if necessary: rescan
    1. Set proper connection settings
    1. Confirm with "OK"
    1. Press "Connect"

2. After successful connection, you will receive some output on the CoolTerm terminal each time a LoRaWAN message is transmitted. You can transmit a message by pressing the physical button on the Adeunis Field Test Device.

3. Enter "Command Mode" to change settings of Adeunis Field Test Device

    In order to change settings of the Adeunis Field Test Device, you have to enter the "Command mode (CM)". Here, I got stuck as I did not know how to append “CR” and ”LF” to the raw hex command as described in the Smartmakers manual. Following steps worked:

    1. Connection (menu) -> Send String
    1. Select "Hex"
    1. Enter "FF FF FF FF FF 2B 2B 2B"
    1. Select "ASCII"
    1. Hit "Enter" at the end of the command
    1. Select "Hex"
    1. Now you see that an "0D 0A" has been attached to the line. Actually, this is “CR” and ”LF”. So next time you can just enter it, instead of switching back to ASCII and hit "Enter".
    1. Press "Send"
    1. CoolTerm should print "CM" in the terminal
    1. Adeunis Field Test Device shows "CM" on the display.

4. Send further commands and change settings of Adeunis Field Test Device

Now, you can send for example the command "AT/V" in ASCII (don't forget to hit enter ("CR" + "LF") after the "AT/V" command) to read the version or "AT/S" in ASCII to read the settings. To change the sending interval use "ATS380=60" in ASCII to set the interval to 60 seconds.
To leave the "command mode" send "AT0" in ASCII.

## Video demo to connect and change settings of Adeunis Field Test Device

<iframe width="560" height="315" src="https://www.youtube.com/embed/mevORtabUOw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
