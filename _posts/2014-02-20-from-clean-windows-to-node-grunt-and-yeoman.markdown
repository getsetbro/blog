---
layout: post
title:  "From clean windows to node, grunt, and yeoman"
date:   2014-02-20 8:00:00
categories:
---
A bit about Node.js and how to some steps to get some experience with it. If you are like me sometimes you can’t quite get your head around something until you start to use it.

Node.js is a platform for server-side and networking applications. Node.js applications are written in JavaScript, and can be run on Windows, Mac and Linux. Node.js includes networking capabilities (like HTTP), and access to the file system. So it sounds like a web server, but it isn't one. To make node.js behave like a web server it needs to be programmed to handle the HTTP requests and provide the responses. Express is the "minimalist web framework" for that. We don’t need Express for the purposes of this article.

Install Node.js from the node website. http://nodejs.org/download/. You can choose the 32bit or 64bit .msi or .exe. For me it installed into C:\Program Files\nodejs\. In a command prompt you can verify this by typing:
{% highlight ruby %}
where node
{% endhighlight %}
The installer will install Node and NPM (a package manager). We will use NPM to install several things.
In a command prompt window type or paste this to install Yeoman:
{% highlight ruby %}
npm install -g yo
{% endhighlight %}
The "-g" will install it globally on your system and not just in the project at hand. Leave off the "-g" if you want it to only install in the current project.
Next install Grunt:
{% highlight ruby %}
npm install -g grunt
{% endhighlight %}
and Bower:
{% highlight ruby %}
npm install -g bower
{% endhighlight %}
Bower is a package manager for the web. Bower runs over Git, and is package-agnostic. Bower depends on Node and npm. It's installed globally using npm.

Make sure that git is installed, as some bower packages require it to be fetched and installed.

To use Bower on Windows, install msysgit from http://git-scm.com/downloads/. Be sure to check the "Run Git from the Windows Command Prompt" option on the Adjusting your PATH environment screen of the install wizard. (If you instead install github-for-windows (http://windows.github.com) it does not add GIT commands to windows command prompt)

Now you have got all of the tools. Use NPM to get a Yeoman webapp generator. A Yeoman generator will add project files to a folder, then Grunt can use those files to build and stuff.
{% highlight ruby %}
npm install -g generator-webapp
{% endhighlight %}
Once that is downloaded run this to set it up:
{% highlight ruby %}
yo webapp
{% endhighlight %}
If instead of the generic webapp generator you wanted the angular generator you would first download it:
{% highlight ruby %}
npm install -g generator-webapp
{% endhighlight %}
and then run it:
{% highlight ruby %}
yo angular
{% endhighlight %}
In the command window you will be asked some questions that you use your keyboard to answer.
"Sass with Compass" needs Ruby installed so deselect that if you do not have Ruby installed. That process can take awhile and when it is done you are back to a prompt with a blinking cursor. If you are not then it did not finish and is not ready to build.

When it does complete you can build and view your project with:
{% highlight ruby %}
grunt serve
{% endhighlight %}
Or just plain:
{% highlight ruby %}
grunt
{% endhighlight %}
for a production build. And to run the tests:
{% highlight ruby %}
grunt test
{% endhighlight %}
