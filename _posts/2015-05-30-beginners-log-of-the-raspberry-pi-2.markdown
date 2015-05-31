---
layout: post
title:  "Beginners Log of the Raspberry Pi 2"
date:   2015-05-30 11:00:00
categories:
---

##Log

* I do not yet have a wifi adapter so I plugged the PI into my router
* On a Mac I tried "ssh pi@raspberrypi.lan" but I got "ssh: Could not resolve hostname raspberrypi.lan: nodename nor servname provided, or not known"
* I could see on my router app the IP of the PI so I was able to connect with "ssh pi@192.168.1.12"
* The default password was "raspberry" for the user "pi"
* I typed "sudo raspi-config" and the config UI showed up in my terminal app

I found this helpful guide for the config menu

> 1. Expand Filesystem: no need to do this — some may disagree on this point
> 2. Change User Password: up to you
> 3. Enable Boot to Desktop/Scratch: by _default_, this is set to console, which is what we want to keep
> 4. Internationalisation Options: set your timezone (if in the US, choose America, then find the correct city with your timezone)
> 5. Enable Camera: _no_ (you can always change this later)
> 6. Add to Rastrack: _no_
> 7. Overclock: this is up to you, I usually choose Medium, which makes the Pi run a little bit faster at the expense of power and potential component damange
> 8. Advanced options: _choose A4 SSH_ - this will enable secure shell access, which means that you can control your Raspberry Pi from a remote computer (extremely useful)

* Number 8 was done for me
* I set the timezone
* I ran "sudo apt-get update && sudo apt-get upgrade"
* I indeed have wheezy and I have ARM hardware as shown by these lines:
{% highlight ruby %}
Hit http://raspberrypi.collabora.com wheezy Release
Hit http://archive.raspberrypi.org wheezy/main armhf Packages
{% endhighlight %}

***

###To open config files in the nano editor

- network "sudo nano /etc/network/interfaces"
- keyboard "sudo nano /etc/default/keyboard"

***

###Log continued...

* When using my keyboard I noticed some characters weren't what I typed
* In the keyboard file I change XKBLAYOUT from "gb" to "us"
* nano is a simple built-in editor. Type in ctrl-X and Y then enter to overwrite to save the file.
* Make these keyboard changes take effect by rebooting: "sudo reboot"

***

###List of commands to know

* power off "sudo poweroff"
* reboot "sudo reboot"
* apt-get update   <--updates the list
* apt-get upgrade    <--installs the updates
* apt-get autoclean   <--cleans up afterwards if necessary
* You can substitute "aptitude" for "apt-get." Aptitude is an Ncurses viewer of packages installed or available. Aptitude can be used from the command line in a similar way to apt-get. See man aptitude for more information.

***

###Setup static IP address

* Get the needed numbers below by typing "ifconfig" and "netstat -nr"
* Typed "sudo nano /etc/network/interfaces"
* Changed this:
{% highlight ruby %}
auto eth0
allow-hotplug eth0
iface eth0 inet manual
{% endhighlight %}
* To this:
{% highlight ruby %}
auto eth0
allow-hotplug eth0
iface eth0 inet static
address 192.168.1.81
netmask 255.255.255.0
broadcast 192.168.1.255
network 192.168.1.0
gateway 192.168.1.1
{% endhighlight %}
* "sudo reboot" - once back in run "ifconfig" to reveal your new settings.


