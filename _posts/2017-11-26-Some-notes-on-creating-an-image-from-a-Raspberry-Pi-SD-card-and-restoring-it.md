---
layout:     post
title:      Creating images from a Raspberry Pi
author:     Christoph Molitor
tags: 		RaspberryPi CheatSheet
subtitle:  	Some notes on creating an image from a Raspberry Pi SD card and restoring it
category:  	report
---
<!-- Start Writing Below in Markdown -->

1.  List all mounted devices:
    ```diskutil list```

    Extract the information which diskX is the one of the SD card.

2.  Unmount device:
    ```diskutil unmountDisk /dev/diskX```

3.  Create Image:
    ```sudo dd if=/dev/diskX of=/Users/.../NameOfImage.img bs=1m```

4.  Restore image to device:
    ```sudo dd bs=1m if=/Users/.../NameOfImage.img of=/dev/diskX```

5.  Compress image:
    ```gzip -fk /Users/.../NameOfImage.img```