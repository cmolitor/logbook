---
layout:     post
title:      A smart gate for a milking robot
author:     Christoph Molitor
tags: 		AgTech Redbear
subtitle:  	Building an IoT prototype for an agtech use case using the Redbear/Particle.io platform in combination with Node-RED
category:  	report
published:	true
date:       2019-05-31
---
<!-- Start Writing Below in Markdown -->

## Lesson's learned
* Observing the user/customer doing a task is more important than just asking him about it
* Particle.io is a great platform for building prototypes and small series devices

## Background

Since a while I am interested in the internet of things and possible applications leveraging the underlying paradigm of connecting a variety of devices and sensors to the internet.
Due to my studies and my work as a researcher I gained experience programming some applications and involving communication between simulators running on different platforms. Futhermore, I had gained experience with the Raspberry Pi due to several work and hobbyist projects.

Thus, it was clear to me that I had to back the Redbear Kickstarter campaign, as the initiators promised with the Redbear Duo a powerful device with various communciation interfaces and yet easy to program.

However, as the funding of the campaign was successful and I received the hardware in August 2016, I was lacking a specific use case. Thus, the hardware was sitting on my desktop waiting for an application for two years. 
This is due to the situation that I rather motivate myself to build something kind of useful with a real function instead of building some blinking leds in combination with a push button.

## Finding my motivating use case

Thus, I needed to find a motivating use case. I found it, when I was visiting my parents which are running a diary. After having breakfast and sitting at a table with my father, he told me that he has to go back to the cowshed to open the gates.
Asking what gates and why he used them, he told me that although he has a fully autonomous milking robot, he as to lock some cows in an area which they can only leave by passing through the robot. 

    Excursion: In general the concept of the farm is that the cows can walk around freely in the barn and visit the milking robot as they wish. And almost all cows do visit the robot to be milked. However, there are a few cows which need a little push to visit the robot. This might be due to that they are new at the farm and don't know the system yet or that they have a bad day;-).

I started to wonder why the fully automated milking robot does not send a notification when all the cows passed the robot. Actually, the milking robot has the functionality to send notifications or make calls, when there is something going wrong. However, here it does not know how many cows have been separated and need to pass, so it cannot send a notification.

So I started thinking that the operator (here my dad) needs a possibility to set (2) a specific number of cows which have to pass the robot and then receives a notification (3) when the cows have passed the robot (1).

The solution which came to my mind to solve the three sub tasks (1,2,3), was the following:

1) Sensing: The milking robot has a entrance gate and an exit gate. When the entrance gate opens the cow can enter the cabin. As soon as the cow entered the cabin, the entrance gate closes again. After finishing the milking, the exit gate opens, the cow can leave the cabin and the exit gate closes again while the entrance gate opens again. So my thought was to put a door or gate contact at one of the gates and count the gate "events".

2) User interface: Often the first thing which comes to my mind is a webinterface or an app to set parameters or set in this specific case a number of cows. However, in this specific evironment, a farm, often gloves are worn or fingers might be wet. Thus, I decided to put a rotary knob and a small display to see the set value. Voice control could be also an option but the environment is also rather noisy.

3) Notification: After the set number of cows has passed the milking robot, the operator, e.g. my father, has to receive a notification, so they can go and open the gates. Here, the simplest solution is to send an e-mail which they can receive on their mobile or PC.

This is more or less the sketch of the solution which I planned to realize. Furthermore, I thought that the Redbear Duo could be a good device to build this solution. The steps to implement the solution are described in detail in the following sections.

## Building a functional prototype on a breadboard

The first step was to connect and setup the RedBear Duo. Here, I followed the official instructions which can be found at 
[Github](https://github.com/redbear/Duo/blob/master/docs/out_of_box_experience.md). In the meantime RedBear has become a part of Particle.io, thus the RedBear App seems to not work anymore. However, you can also use a serial connection and a terminal to setup the device.

Although the RedBear Duo can be used withput the Particle.io platform, I decided to register at Particle.io in order to try out the platform, mainly the Particle Web IDE.
After setting up a Particle.io account, I could claim my RedBear Duo device on the Particle.io platform.

The Particle Web IDE allows you to manage your devices, projects and used libraries easily. See here some screen shots.

![Particle Web IDE - Apps](/assets/readbearduo/particleio.png)

![Particle Web IDE - Library Browser](/assets/readbearduo/particleio_lib.png)

After setting up the RedBear Duo and the particle Web IDE, I started to develop the code while adding the external hardware (here: display, button and rotary knob) one after the other.

The latest version of the code, I developed can be found [here at Github](https://github.com/cmolitor/lely_smartgate/tree/master).

The final breadboard setup looks like depicted here:
![Breadboard](/assets/readbearduo/setup_breadboard.jpg)

In accordance with the previous described substasks, I proceeded the following way:

1. **Sensing: Started with a push button as event trigger, later replaced by door contact**

    At the beginning for getting started I used a typical push button to emulate "gate opened" events. The push button has been connected to the RedBear Duo digital input pin using a voltage divider circuit. Later on, after finishing the rest of the whole setup, I replaced the push button with a door contact which is also used in the deployed setup.

    I got the following door contact made of stainless steel: [Door contact](https://www.ebay.de/itm/Edelstahl-Sicherheits-Sturm-Tuer-magnetischer-Reedschalter-Kontakt-Alarm-R7N7/182860763961)

2. **User interface: rotary knob and display**

    As user interface, I decided to use a display to visualize the set values and the current status. Furthermore, a rotary knob has been used to set the values (here number of cows) of the device. 

    1. Display:
        As display I used a 4 digit 7 segment display which is typically used for clocks. I got mine here at [Aliexpress](https://de.aliexpress.com/item/Free-shipping-4-digital-display-with-adjustable-brightness-LED-module-clock-Point-Accessories-Blocks-for-arduino/1889678421.html). 

        The library for controlling the display is already available in the Particle Web IDE, just search in the library tab for "TM1637Display".

        However, I used a slightly changed library from [Github](https://github.com/avishorp/TM1637/blob/master/TM1637Display.cpp) which allowed better control of the colon. Instead of showing hours and minutes, I used the left two digits of the display (hours display) to show current values and the right two digits of the display (minutes display) to show set values. More details can be found further down.

    2. Rotary knob:
        In order to set the values of the device, I decided to use a rotary knob which can be also used with gloves and is rather insensitive to dirt and humidity. 

    The user interaction through the user interface follows the following description: At startup the device displays **0:0**. While the user turns the rotary knob, the left side of the display (hour display) is updated according to the rotation, eg. to **5:0**. In order to set the displayed value as value to count down, the user pushes the rotary knob. On the push of the rotary knob, the set value, here **5** is displayed at the right side of the display (minutes display) while the left side (hour display) is set back to **0**.

    Form this moment on, the device also counts the gate open events starting from zero and displays the current number of gate open events on the left side (hour display). 

    <!-- 
        <div class="mermaid">
            graph TD;   
                A[Turn rotary knob] ==> B[Push rotary knob at desired set value] ==> C[Reset gate open events and start counting];
        </div> 
    -->

3. **Notification**

    The last task which had to be done in order to provide a valuable solution is to send a notification to the user/operator that all the cows have passed the milking robot. Here I decided that the RedBear Duo sends an MQTT notification to a Node-RED flow which triggers an e-mail to specified recipients. The MQTT message also contains the number of set cows which is used in the e-mail (see screenshot of Node-RED flow).

    The Node-RED flow is depicted here:
    For triggering the e-mail a specific topic has to be triggered. Furthermore, the RedBear Duo client has a unique user and password for connecting to the MQTT server.

    Here, I was using my already existing Node-RED setup. However, also the use of a service like IFTTT or others could be an alternative solution.

    ![Flow in Node-RED](/assets/readbearduo/nodered_flow.png)

    <img src="/assets/readbearduo/nodered_composeemail.png" alt="Code of Compose Email Node in Node-RED" style="width: 80%"/>
    

## Installing all components in a deployable case

After developing a functional setup, comprising hardware and software, on a breadboard, a deployable setup had to be developed.
Deployable setup here means that all the hardare is installed and mounted in way that it can used in a harsh environment like a diary farm. 
In order to install the hardware, I ordered the following [case](https://www.amazon.de/dp/B06XR3FHJ8/ref=pe_3044161_185740101_TE_item).

Within this case, I mounted a stripboard as base plate to hold the RedBear RBLink which serves as break out board for the RedBear Duo. Furthermore, the voltage divider for the input of the door contact is soldered on this stripboard.

Furthermore, I drilled three holes in the case. One for mounting the wifi antenna, one for a 4-pole connector to provide connection for the power supply and the door contact and the third hole for mounting the rotary knob. As connector, I used the following [connector](https://www.amazon.de/Gazechimp-Männlicher-Steckverbinder-Kabelstecker-Modellnummer/dp/B01M1CQTXP/). However, for the next time I would prefer a different type of connector which provides an easier way to connect cables to.

The 4 segment display was fixed to the transparent cover of the case with a transparent double-sided tape.

The fully mounted case can be seen in the pictures and video below.

## Deployment

After mounting all the components in the deployable case, it is time to put the whole device in the field.

First the door contacts are mounted on the gate through which the cows leave the milking robot. See the picture below.

![Mounted door contacts](/assets/readbearduo/mounting_door_contact.jpg)


After this the case with user interface consisting of the display and the rotary knob is mounted at the housing of the milking robot where it can be easily reached by the user.

![Monuted case](/assets/readbearduo/mounting_case.jpg)

The following video demonstartes the setting of the number of cows.
<iframe width="560" height="315" src="https://www.youtube.com/embed/pnrxK1ZnHJY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Long term operation

As I am writing finally this report and documentation the "Lely Smartgate", as I call the device, is already in stable operation since 5 month. So far the device is running really stable and did not show any malfunction so far. The detection of the "gate open" events works so far without any reported error.

Once, I updated the software, in order to add the number of cows which have past the gate into the e-mail. The update of the software worked seemlessly through the Particle.io WebIDE.

Furthermore, the device is actually really used on a daily basis and provides a real use for my dad. 

## Conclusion and lesson learned

- Goal was to build a useful use case using the RedBear Duo. 
- Using the RedBear Duo was fun. As it is depreceated, I hope the Particle Photon is similar to use for the next project.
- Programming the RedBear Duo via the Particle WebIDE works like a charm and is really fun.
- While building the device, I once showed it to my dad and he wasn't sure if it is really useful. However, now the device is frequently used and provides value, thus, the lesson learned is:
    - prepare better demos
    - don't ask user what he wants, better observe how he does certain tasks and think what could be improved 


## Alternatives and other solutions and todos


- get data from milking robot instead of installing a door contact
- use IFTTT instead of Node-RED instance
- ask other robot users if this feature would be interesting for them
