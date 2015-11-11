---
layout: post
title:  "Windows 10 UWP apps Q's and A's"
date:   2015-08-19 11:00:00
categories:
---

###Q: How do you start a JS win10 uwp ​​app?​
A: It starts with a default html page (no CSS needed) and a default JS file. The JS file has boilerplate code to initialize the app and get the basic controls running.

---

###Q: How do you add a page?​​​​​​
A: -PageControls- are 3 files (html, css, js) and should go in their own folder in the PAGES folder:

1. pages / home / home.html
2. pages / home / home.css
3. pages / home / home.js

---

###Q: How do navigate from page to page?​​​
A: Program the links to run
{% highlight ruby linenos %}WinJS.Navigation.navigate( "/pages/page2/page2.html" );​​{% endhighlight %}
{% highlight ruby linenos %}WinJS.Navigation.navigate(location, initialState).done();​​{% endhighlight %}
---

###Q: How do I layout items so they fit and scroll like a native app
A: With custom CSS3, using things like flexbox or height: calc( 100% - 20px ​)

---

Comments can happen here: [/blog/issues/3](https://github.com/getsetbro/blog/issues/3)