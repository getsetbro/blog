---
layout: post
title:  "Stop paying your SharePoint jQuery Tax"
date:   2012-08-20 8:00:00
---
Stop paying your jQuery tax by Sam Saffron is a great post about why but also how to put external JavaScript includes at the bottom of the page. This is an important how-to for performance but even more critical for developers of a CMS system like SharePoint. In SharePoint you want to be able to open a page in edit-mode and drop in some dynamic content. But to add some jQuery code in the middle of the page you need the jQuery JS file to be referenced above it. For this most developers will put a link to jQuery.min.js in the HEAD of the site. WRONG. This is where it is important to use a mechanism like what the article describes so you can get both the performance of external JS includes at the bottom and the ability to add JS in the middle of your page.

Here are some simplified steps of how you will want to accomplish it:

1. Put a line of JS in the HEAD of your site that sets up the JavaScript array. This will collect all of the JS that you put in the middle of your page:

{% highlight ruby linenos %}
<script type='text/javascript'>
  window.q=[];
  window.$=function(f){
    q.push(f);
  }
</script>
{% endhighlight %}
2. Put some JS in the middle of your page in this format:

{% highlight ruby linenos %}
<div id="jQueryTest">Content BEFORE jQuery code has selected and changed it.</div>
<script type="text/javascript">
  function MyTest(){
    $("#jQueryTest").html("Content AFTER jQuery code has selected and changed it.");
  }
  q.push(MyTest);
</script>
{% endhighlight %}
Each time that you add JS code to the middle of the page you push it into the array.

3. At the bottom of the site link to jQuery JS and after that add a function that loops through the array and runs it:

{% highlight ruby linenos %}
<script type="text/javascript" src="jquery.min.js"></script>
<script type="text/javascript">
  $.each(q,function(i,f){
    $(f);
  });
</script>
{% endhighlight %}

---

Comments can happen here: [/blog/issues/15](https://github.com/getsetbro/blog/issues/15)