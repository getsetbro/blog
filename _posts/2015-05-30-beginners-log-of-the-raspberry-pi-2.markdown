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

###Log continued...

* I want to play sound through the audio jack
* Check to see if sound is ready
{% highlight ruby %}"lsmod | grep snd_bcm2835"{% endhighlight %}
* It shows bcm2835 is there
* Grab a WAV file "wget http://www.freespecialeffects.co.uk/soundfx/computers/bleep_01.wav"
* Play it using "aplay bleep_01.wav"

Play mp3 files

* To play mp3s "sudo apt-get -y install mpg321"
* Get an mp3 "wget http://www.freespecialeffects.co.uk//soundfx/various/fire_burning.mp3"
* Play "mpg321 fire_burning.mp3"
* Play with volume at 50% "mpg321 -g 50 fire_burning.mp3"

Installing NodeJS

* Latest stable node.js package available from the node-arm site "wget http://node-arm.herokuapp.com/node_latest_armhf.deb"
* Install it "sudo dpkg -i node_latest_armhf.deb"
* Check to see if we get a version number "node -v" it says "v0.12.1"
* Check NPM "npm -v" it says "2.5.1"

Using NodeJS

* Another check to see Node working is to type "node" and then at the prompt "new Date()" it says "Sun May 31 2015 09:25:09 GMT-0500 (CDT)"
* After SSH we are in /home/pi folder
* Make a directory called node_programs "mkdir node_programs"
* Change directory to node_programs "cd node_programs"
* Make and change to new folder helloworld "mkdir helloworld && cd helloworld"
* Initialize a new node project here "npm init" and click enter through all the prompts
* Start an empty file named index.js "nano index.js" - the nano editor opens to a blank file.
* CTRL + X to exit, Y selects Yes, and Enter completes and closes it.

Use nodejs to play a mp3

* "npm install mpg321" in the node program folder created above
* Put an mp3 in the current folder "wget http://www.freespecialeffects.co.uk//soundfx/various/fire_burning.mp3"
* Open to edit the index.js file "nano index.js"
* Paste the following:

{% highlight ruby %}
var mpg321 = require('mpg321');

var proc = mpg321()
  .loop(0) // infinity loop
  .file('./fire_burning.mp3')
  .exec();

// SIGINT hack - stops running on ctrl + C in the terminal
process.on('SIGINT', function (data) {
  process.exit();
});
{% endhighlight %}

* Run the index.js file with node "node index.js"
* The mp3 plays through the audio jack

***

###For setting a static IP address

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


