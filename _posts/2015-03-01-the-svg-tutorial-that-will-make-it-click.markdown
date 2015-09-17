---
layout: post
title:  "The SVG tutorial that will make it click"
date:   2015-03-01 13:00:00
categories:
---

When you want a shape in your webapp that is responsive and looks sharp at any size SVG is the way to go but where would you start? Here are some steps that can get you something useable and hopefully make some concepts around SVG click for you. There is much more to it, of course, but this might get you off and running.

- Optional: Logon to your favorite web browser
- Head to an online SVG generator such as [//editor.method.ac](//editor.method.ac)
  - Half way down, on the left, choose a shape from the shape library.
    - Some shapes have a lot of code behind them so choose a simple one like the triangle.
- Now draw it on the canvas with the shift key pressed so I keeps a square ratio for the width and height. 

How big to draw? For this lets make it around 200 X 200. A big SVG object can have a lot of nice detail. But if the page ever lost the CSS that tells the large SVG to be a small icon then your visitor will be looking at a page full of SVG.

Notice that under the view menu you can see the source of the shape that you just drew.

- Before you export be sure to change the canvas size to match the content.
  - This makes it so we don't have extra unused space in our file.
    - and we will be able to tell what size the shape is by the width and height shown in the SVG attributes. You will see what I mean.
  - Make sure no SVG shapes are selected by choosing the Arrow tool and clicking outside of the shape and seeing the selection box goes away.

- On the right, under canvas, choose "Fit to Content" from the sizes drop-down.
  - If it already on "Fit to Content" you need to select something else and then again choose "Fit to Content"
- View the source
  - from the View menu and copy the code of the SVG file.
- Now paste it in a new bin somewhere like [//jsbin.com](//jsbin.com)
  - What you have is static sized SVG shape that does not resize. Next we'll fix that.
- The beginning of your SVG code looks like this
<pre>&lt;svg width=&quot;189&quot; height=&quot;165&quot;...</pre>
- This is the width and height of the contents.
  - Add the viewBox attribute to the SVG element using the width and height attribute values:
<pre>&lt;svg width=&quot;189&quot; height=&quot;165&quot; viewBox=&quot;0 0 189 165&quot;&gt;</pre>
- Now change the width and height to 100%
<pre>&lt;svg width=&quot;100%&quot; height=&quot;100%&quot; viewBox=&quot;0 0 189 165&quot;&gt;</pre>
- You now have a shape that scales and responds to its confines.
  - Now the problem becomes what its confines are
- Understand how the viewBox wants to use the width and height of the contents inside of it.
  - If you use numbers bigger than the contents you will have wasted space.
    - If you use numbers that are smaller than the contents you will cut off part of the contents.
- Try putting it into a bootstrap grid and see what you've got


{::nomarkdown}
<iframe src="http://jsbin.com/dogeno/1/embed?output" style="border: 1px solid rgb(170, 170, 170); width: 100%; min-height: 330px; height: 330px;"></iframe>
<iframe src="http://jsbin.com/dogeno/1/embed?html" style="border: 1px solid rgb(170, 170, 170); width: 100%; min-height: 330px; height: 330px;"></iframe>
{:/nomarkdown}

Notice that I stripped everything but the SVG and the PATH. The Title and G elements have there place but for this we are not in need of them.

I wont go into what this tag does but it's a good thing: preserveAspectRatio="xMidYMid meet"

To put it simply, to make a SVG element responsive the width & height move to the viewBox & change them to 100%.

<img src="//pbs.twimg.com/media/B-4asHkUcAE42Kv.png" alt="svg"/>
