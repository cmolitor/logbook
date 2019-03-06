---
layout:     post
title:      Changing Wifi credentials of Redbear Duo IoT board
author:     Christoph Molitor
tags: 		RedBear IoT CheatSheet
subtitle:  	Description how to change wifi settings of RedBear Duo
category:  	report
---
<!-- Start Writing Below in Markdown -->

At some point I had to change the Wifi credentials of my Redbear Duo board. Following the [official instructions](https://github.com/redbear/Duo/blob/master/docs/out_of_box_experience.md), I was not able to change the credentials with the Android app. Probably as the app is not anymore maintained.

Thus, I followed the instructions to use a USB connection and the terminal on macOS to set the Wifi credentials.

Those were the steps:

1) Connect the Redbear Duo with the USB cable to my MacBook

2) Push the setup button at the Redbear Duo for at least ten seconds, until it starts flashing very fast. After around three seconds the blue led starts flashing and the Redbear Duo is in Listening mode, but in order to reset the wifi credentials you have to press the button longer until the blue led starts flashing very fast.

3) Search and connect to Redbear Duo:

search for devices:
```ls /dev/tty.usbmodem*```

connect to device:
```screen /dev/tty.usbmodemXXXXX```


4) use commands: 
```i: show id```
```v: show version```
```w: setup wifi```

5) Done!
