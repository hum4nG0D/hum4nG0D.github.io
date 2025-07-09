---
title: "Creating A $3 DIY RubberDucky"
description: "Using DigiSpark's ATtiny85 to create an effective RubberDucky."
date: 2021-06-22
categories: [PoC]
tags: [windows, rubberducky, red team, penetration testing, physical security]
pin: true
math: true
mermaid: true
image:
  path: "/assets/img/rubberducky/rubberducky.png"
  alt: "Creating $3 DIY RubberDucky"
---

Creating a disposable DIY rubberducky for your Red Team engagement or at least testing out what you can achieve with 1 minute of physical access to the computer. This is the first post of a DIY series. Hopefully, I will be able to create subsequence posts around DIY rubberducky. In this section, I will be going through the setup and where you can get ATtiny85.

Where to get DigiSpark ATtiny85:

Buying options: [Lazada](https://www.lazada.sg/products/5pcs-attiny85-digispark-i2c-led-rev3-kickstarter-5v-iic-spi-usb-development-board-6-io-pins-for-arduino-i1978909921-s10693197391.html?spm=a2o42.searchlist.list.1.5ea345denMg8T0&search=1&freeshipping=1) 5pcs - S$15, [Amazon](https://www.amazon.com/Teyleten-Robot-Digispark-Kickstarter-Development/dp/B08HCR1CKX/ref=sr_1_2?dchild=1&keywords=AiTrip+5pcs+Digispark&qid=1633838802&sr=8-2) 5pcs - US$12

What you will need:

Arduino IDE: [https://www.arduino.cc/en/software](https://www.arduino.cc/en/software)

DigiSpark Driver: [https://github.com/digistump/DigistumpArduino/releases](https://github.com/digistump/DigistumpArduino/releases)

<br>

### Installing Arduino IDE

Go to the above link and download the IDE for your enviroment. In this case I am downloading Windows 10 x64 version. You can also get Arduino IDE from Microsoft Store but that's totally up to you. 

![DIY RubberDucky](/assets/img/rubberducky/idedownload.png)

After downloading, go ahead and install the IDE. After that download the driver and install it by running `Install Drivers.exe`. 

![DIY RubberDucky](/assets/img/rubberducky/driverdownload.png)

After installing the driver, you will be able to see the ATtiny85 as `Digispark Bootloader` in the Device Manager.

![DIY RubberDucky](/assets/img/rubberducky/devmgmt.png)

<br>

### Setting Up

Fire up Arduino IDE. You will be seeing something like below.

![DIY RubberDucky](/assets/img/rubberducky/arduino01.png)

Before we can proceed, we'll need to install Board Manager for our Digispark ATtiny85. Go to `FIle > Preferences`. 

![DIY RubberDucky](/assets/img/rubberducky/arduino02.png)

In the `Addional Boards manager URL` field, put in the link: `http://digistump.com/package_digistump_index.json` like so.

![DIY RubberDucky](/assets/img/rubberducky/arduino03.png)

Press `OK`. Then go to `Tools > Boards > Boards Manager...` and in the search field, type in `Digistump` where you will see `Digistump AVR Boards`. Install that if you haven't already.

![DIY RubberDucky](/assets/img/rubberducky/arduino04.png)

Finally, go to `Tools > Boards` and choose `Digispark (Default - 16.5mhz)` from `Digistump AVR Boards`. 

![DIY RubberDucky](/assets/img/rubberducky/arduino05.png)

And choose `Programmer` as `Micronucleus`.

![DIY RubberDucky](/assets/img/rubberducky/arduino06.png)

That's it. You are good to go now. In the next post we will be adding payloads in our DIY RubberDucky. For any questions, feel free to reach out to me on twitter.