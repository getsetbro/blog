---
layout: post
title:  "From CSS2 to Compass / Sass"
date:   2014-08-28 13:34:54
categories:
---
If you have been seeing talk of CSS extensions like LESS and SASS but have not had a chance to use them then this article might give you an idea of what it would be like.

First I will show what it was like using CSS 2 before CSS 3 came along.

##CSS 2:
{% highlight ruby linenos %}
.box{
  border: 1px solid gainsboro;
}
{% endhighlight %}
Wee. No wait. Boring.

Then CSS3 came and we can do great stuff like round corners.

##CSS 3:
{% highlight ruby linenos %}
.box {
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  -ms-border-radius: 10px;
  -o-border-radius: 10px;
  border-radius: 10px;
}
{% endhighlight %}
But look at all those pre-fixes. So with SASS we can define it once and use it many many times.

##SASS:
{% highlight ruby linenos %}
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
  -moz-border-radius: $radius;
  -ms-border-radius: $radius;
  -o-border-radius: $radius;
  border-radius: $radius;
}

.box1 { @include border-radius(10px); }
.box2 { @include border-radius(12px); }
.box3 { @include border-radius(14px); }
{% endhighlight %}
The output is converted to be just like the CSS3 above that was all written by hand:
{% highlight ruby linenos %}
.box1 {
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  -ms-border-radius: 10px;
  -o-border-radius: 10px;
  border-radius: 10px;
}
.box2 ...
{% endhighlight %}
But then with Compass (which uses SASS) we don't have to write the MIXINs. They are written for us. We can just include the set of MIXINs we want and then start using them like this...

##Compass:
{% highlight ruby linenos %}
@import "compass/css3";

.box {
  @include border-radius(10px);
}
{% endhighlight %}

---

Comments can happen here: [/blog/issues/11](https://github.com/getsetbro/blog/issues/11)