---
layout:     post
title:      Building an Agtech prototpye
author:     Christoph Molitor
tags: 		AgTech Redbear
subtitle:  	Experience on building an IoT prototype for an agtech use case using the Redbear/Particle.io platform in combination with Node-red
category:  	report
published:	true
---
<!-- Start Writing Below in Markdown -->


= Experience on building an IoT prototype for an agtech use case using the Redbear/Particle.io platform in combination with Node-red

Since a while I am interested in the internet of things and possible applications leveraging the underlying paradigm of connecting a variety of devices and sensors to the internet.
Due to my studies and my work as a researcher I gained some experience programming some applications and involving communication between simulators running on different platforms but also involving the RaspberryPi.

Thus, it was clear to me that I had to back the Reabear Kickstarter campaign, as the initiators promised with the Redbear Duo a poweful device with various communciation interfaces and yet easy to program.

However, as the funding of the campaign was succesful and I received the hardware after a while, I was lacking a specific use case. Thus, the hardware was sitting on my desktop waiting for an application. This is due to the situation that I rather motivate myself to build something kind of useful with a real function instead of building some blinking leds in combination with a button you can push.

== Finding my motivating use case

Thus, I needed to find a motivating use case. I found it, actually without actively looking for it, when I was visiting my parents which are running a diary. After having breakfast and sitting at a table with my father, he told me that he has to go back to the barn to open the gates.
Asking what gates and why he used them, he told me that although he has a fully autonomous milking robot, he as to lock some cows in an area whcih they can only leave by passing through the robot. 

Excursion: In general the concept of the farm is that the cows can walk around freely in the barn and visit the milking robot as they wish. And almost all cows do visit the robot to be milked. However, there are a few cows which need a little push to visit the robot. This might be due to that they are new at the farm and don't know the system yet or that they are sick.




##

sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 443

iptables -t nat -L --line-numbers

# Delete routin in line number x (here: 1)
iptables -t nat -D PREROUTING 1


## get certificate (port 80 has to work for temporary webserver)
sudo certbot certonly
