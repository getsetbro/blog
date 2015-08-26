---
layout: post
title:  "Webapp on Raspberry Pi via the Windows UWP"
date:   2015-08-25 16:00:00
categories:
---
<!-- 
bold
**bold**
 -->
![Windows 10 IOT boot image](http://getsetbro.com/images/onraspiviawinuwp/win10iot.png)

I mentioned in the last post I was going to try deploying our "Vacation Planner" web application to a Raspberry PI. I did try it and it did work. The web-app didn't need any changes to run on the device. So while I don't have a list of tweaks needed to make a webapp work on IOT devices I can tell you about some of the gotchas I encountered getting to the place where I could install the web-app.

If you are unfamiliar with the Windows UWP bridge it's toolkit enables "you to quickly bring your existing applications to the Universal Windows Platform and Windows Store." It wraps web-apps and deploys it to multiple devices (PCs, tablets, phones, HoloLens, Surface Hub, Xbox, and Raspberry Pi).

![Windows 10 Devices](http://getsetbro.com/images/onraspiviawinuwp/win10devices.png)

First thing is to get win10 IOT operating system on to a sdcard for the raspberry Pi to run from. I already had a build from a couple of months ago but that became a major obstacle when an error told me the debugger version on the device did not match the version of VS on my dev machine. As of today the correct steps are listed here [ms-iot.github.io/content/en-US/win10/SetupRPI.htm](http://ms-iot.github.io/content/en-US/win10/SetupRPI.htm)

With the OS on the sdcard the Raspberry Pi will boot up and grab an IP address over a wired network connection. No wifi support at this point, which is a big barrier for many who wont feel like running a long network cable through their house or office. I, myself, do not mind.

<aside class="thumbnail pullright">
  <img src="http://getsetbro.com/images/onraspiviawinuwp/winservices.png" alt="Windows services list">
</aside>
With the Pi on the network you can connect to it from your dev machine. Make sure the Windows Remote Management service is running on the dev system. For me it was off so I had to start it. I chose to have it always start automatically.

---

Scott Hanselman's article is accurate for the commands to connect and login: [hanselman.com/blog/SettingUpWindows10ForIoTOnYourRaspberryPi2.aspx](http://www.hanselman.com/blog/SettingUpWindows10ForIoTOnYourRaspberryPi2.aspx). He had all the steps I needed except he was deploying a C# app which has a different manual config screen than what I needed for my remote web-app. For me when I selected the properties of my project it looked like this:

![Project properties](http://getsetbro.com/images/onraspiviawinuwp/projectproperties.png)

In your setting choose not to use authentication. (shown here [oliviaklose.com/hello-blinky/](http://oliviaklose.com/hello-blinky/) )

Be aware that to the credentials for the Pi the username is "MINWINPC\administrator" and password is "p@ssw0rd" - some articles I saw did not have the machine name as part of the username in their instructions.

The device is serving up a webpage that can be provide tasks at http://IP.AD.DRE.SS or http://minwinpc (if and when it works). Once you have deployed you will go to this webpage to START your app - using the webpage interface.

The remote machine IP address can get stuck - I had to create a new app when the IP of my device changed.

##Photo gallery
![End result](http://getsetbro.com/images/onraspiviawinuwp/kiosk.png)

###Date Picker
![Date picker in effect](http://getsetbro.com/images/onraspiviawinuwp/datepicker.png)

###Deployed
![The deployed code output](http://getsetbro.com/images/onraspiviawinuwp/deployed.png)

---
[ Words: {{ page.content | number_of_words }} ]