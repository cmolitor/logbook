---
layout:     post
title:      Building an Agtech prototpye
author:     Christoph Molitor
tags: 		AgTech Redbear
subtitle:  	Experience on building an IoT prototype for an agtech use case using the Redbear/Particle.io platform in combination with Node-red
category:  	report
published:	false
---
<!-- Start Writing Below in Markdown -->


= Experience on building an IoT prototype for an agtech use case using the Redbear/Particle.io platform in combination with Node-red

Lesson's learned
* Observing the user/customer doing a task is more important than just asking him about it

Since a while I am interested in the internet of things and possible applications leveraging the underlying paradigm of connecting a variety of devices and sensors to the internet.
Due to my studies and my work as a researcher I gained experience programming some applications and involving communication between simulators running on different platforms. Futhermore, I had gained experience with the RaspberryPi due to several work and hobbyist projects.

Thus, it was clear to me that I had to back the Redbear Kickstarter campaign, as the initiators promised with the Redbear Duo a powerful device with various communciation interfaces and yet easy to program.

However, as the funding of the campaign was successful and I received the hardware in August 2016, I was lacking a specific use case. Thus, the hardware was sitting on my desktop waiting for an application for two years. 
This is due to the situation that I rather motivate myself to build something kind of useful with a real function instead of building some blinking leds in combination with a push button.

== Finding my motivating use case

Thus, I needed to find a motivating use case. I found it, when I was visiting my parents which are running a diary. After having breakfast and sitting at a table with my father, he told me that he has to go back to the barn to open the gates.
Asking what gates and why he used them, he told me that although he has a fully autonomous milking robot, he as to lock some cows in an area which they can only leave by passing through the robot. 

Excursion: In general the concept of the farm is that the cows can walk around freely in the barn and visit the milking robot as they wish. And almost all cows do visit the robot to be milked. However, there are a few cows which need a little push to visit the robot. This might be due to that they are new at the farm and don't know the system yet or that they have a bad day;-).

I started to wonder why the fully automated milking robot does not provide send a notification when all the cows passed the robot. Actually, the milking robot has the functionality to send notifications or make calls, when there is something going wrong. In this case, it does not know how many cows have been separated and need to pass, so it cannot send a notification.

So I Started thinking that the operator (here my dad) needs a possibility to set (2) a specific number of cows which have to pass the robot and then receives a notification (3) when the cows have passed the robot (1).

The solution which came to my mind to solve the 3 sub tasks, was the following:

1) The milking robot has a entrance gate and an exit gate. When the entrance gate opens the cow can enter the cabine. As soon as the cows entered the cabine, the entrance gate closes again. After finishing the milking, the exit gate opens, the cow can leave the cabine and the exit gate closes again while the entrance gate opens again. So my immediate thought was to put a door or gate contact at one of the gates and count the gate "events".

2) User interface: Often the first thing which comes to my mind is a webinterface or an app to set paramters or set in this specific case a number of cows. However, in this specific evironment, a farm, often gloves are worn or fingers might be wet. Thus, I decided to put a rotary knob and a small display to see the set value. Voice control could be also an option but the environment is also rather noisy.

3) Notification: After the set number of cows has passed the milking robot, the operator, here my father or my sister, have to receive a notification, so they can go and open the gates. Here, the simplest solution is to send an e-mail which they can receive on their mobile or PC.

This is more or less the sketch of the solution I planned to realize with the Redbear Duo. To realize this project, I proceeded the steps described in detail in the following sections.

== Steps

1. Breadboard








##
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 3000

sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 443

#
iptables -t nat -L --line-numbers

# Delete routing in line number x (here: 1)
iptables -t nat -D PREROUTING 1


## get certificate (port 80 has to work for temporary webserver)
sudo certbot certonly
