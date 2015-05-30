---
layout: post
title:  "Beginners Log of the Raspberry Pi 2"
date:   2015-05-30 11:00:00
categories:
---

##Log

- I do not yet have a wifi adapter so I plugged the PI into my router
- On a Mac I tried "ssh pi@raspberrypi.lan" but I got "ssh: Could not resolve hostname raspberrypi.lan: nodename nor servname provided, or not known"
- I could see on my router app the IP of the PI so I was able to connect with "ssh pi@192.168.1.12"
- The default password was "raspberry" for the user "pi"

.

- I typed "sudo raspi-config" and the config UI showed up in my terminal app
- I set the timezone
- I ran "sudo apt-get update && sudo apt-get upgrade"
- I indeed have wheezy and I have ARM hardware as shown by these lines:
- - Hit http://raspberrypi.collabora.com wheezy Release
- - Hit http://archive.raspberrypi.org wheezy/main armhf Packages

