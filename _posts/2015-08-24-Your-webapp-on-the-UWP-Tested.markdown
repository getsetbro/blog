---
layout: post
title:  "Your web-app on the Universal Windows Platform - Tested"
date:   2015-08-24 10:00:00
categories:
---

##…and found just-working (TL;DR)

Have you seen that you can build great Windows apps with your existing websites? You can take an existing web application and package it for all Windows-based devices including PCs, tablets, phones, HoloLens, Surface Hub, Xbox, and Raspberry Pi (running Windows).

We (my mobility team at [bluemetal](http://www.bluemetal.com)) wanted to test this out and see what the experience was like. Does it just-work or does take a lot of tweaks and massaging to make it usable?

## The web-app we used
As a plain web-app the "Vacation Planner" that we used is a single page built with HTML5, CSS3, and JavaScript. The data entered into the app is stored in the localStorage of the browser. The function of the app is to give users a way to track and keep track of their own time-off and holiday data year over year.

The UI is responsive, it works at any width and will adapt when the boundaries are resized. Make sure your web-app is responsive, it is going to need to flex to any width as it gets deployed across the Windows devices.

For this article we tested the "Vacation Planner" as a Windows desktop app. I hope to write next about the success we had putting on a Raspberry Pi.

## The web-app in the UWP
With the uwp **Bridge** (Project Westminster) the web-app is wrapped as a Windows Native app. When the app is opened on Windows 10 it contains all of the web-app functions but it is enhanced with some Windows only features: Live Tile updates, Active Notifications, Cortana integration, and date-pickers.

## How we did it
The web-app was built for the web with web code, as stated above. It was then deployed online, hosted with github-pages, if you care to know. It lives there and can be viewed and used from any modern-ish browser.

First step was to consume it as a uwp and test it without any Windows specific enhancements. That step is well documented here:
[steps-to-convert-a-website-into-an-web-app](http://microsoftedge.github.io/Web-AppsDocs/en-US/win10/CreateHWA.htm#follow-these-steps-to-convert-a-website-into-an-web-app-on-windows).

The gist of it is you empty out your solution in Visual Studio and point it the URL of your web-app.
![alt text](http://getsetbro.com/images/yourapponuwp/sln.png "Empty VS Solution")
I hope you notice that there is not much there, and there is not going to be. It's still on github-pages, or wherever you host yours, and it will stay there. The beauty of it is that changes can continue to be made to the source and the uwp app will pull down the latest code. No need to re-submit your app to the Windows Store.

## Land and expand
Once we saw the web-app was just as usable in it's uwp app version we could start to enhance it with the Windows API. We integrated date-pickers, Live Tile updates, Active Notifications, and Cortana.

### Date Picker
A quick hit was to get the date pickers working. For that we just made sure that our input fields in HTML had "date" as their type. Any date-picker polyfills should be deactivated.

Example:
{% highlight ruby %}
<input type="date" />
{% endhighlight %}

![alt text](http://getsetbro.com/images/yourapponuwp/dateinput.png "Native Windows date-picker")

---

#### Using the Windows API in your web-app JS
Like I have indicated, integrating these platform utilities works as promised but they should be hidden from browsers. You only want Windows to find them. So with this IF statement you can keep your Windows API code from breaking in browsers:
{% highlight ruby %}
  if (typeof Windows !== 'undefined' && typeof Windows.UI !== 'undefined')
{% endhighlight %}
Only a Windows platform will pass the IF test and continue into the code.

### Live Tile
We got the code for the Live Tile from:
[gist.github.com/seksenov/...](https://gist.github.com/seksenov/5270d534fad70e98054b) and added a button to the app to run the "updateTile" function.

Guess what... it worked.
![alt text](http://getsetbro.com/images/yourapponuwp/liveTile.png "Live Tile")

### Active Notifications (Toasts)
Getting this going was as easy as copy and pasting from an example that devs from the Windows team had posted:
[gist.github.com/seksenov/...](https://gist.github.com/seksenov/2a08ea82483a0578d1aa).

Yes it worked ootb but we decided to only show the toast if Cortana had detected and captured a phrase with speech recognition
![alt text](http://getsetbro.com/images/yourapponuwp/toast.png "Toast")

### Cortana
Getting Cortana working took a few more steps than the others, there are more files involved. Luckily [Kiril Seksenov](https://twitter.com/k_seks) had examples of these too, you might want to bookmark his gist page so you see all of them: [gist.github.com/seksenov](https://gist.github.com/seksenov). There is one for media capture that we didn't use yet.

#### Steps

1. Add a vcd.xml file to your site [gist.github.com/seksenov/...#file-vcd-xml](https://gist.github.com/seksenov/17032e9a6eb9c17f88b5#file-vcd-xml)
2. Point to the vcd.xml file in the head of your web-app [gist.github.com/seksenov/...#file-cortana-html](https://gist.github.com/seksenov/17032e9a6eb9c17f88b5#file-cortana-html)
3. Do something in your app when the commands come in from Cortana [gist.github.com/seksenov/...#file-cortana-js](https://gist.github.com/seksenov/17032e9a6eb9c17f88b5#file-cortana-js)

![alt text](http://getsetbro.com/images/yourapponuwp/cortana.png "Cortana")

## Next

### Windows 10 IOT

Next we will install the Vacation Planner UWP app on a Raspberry Pi running Windows 10 IOT. Wish us luck and check back for the results.

---

Comments can happen here: [/blog/issues/1](https://github.com/getsetbro/blog/issues/1)
