---
layout: post
title:  "CSS IS AWESOME or DIVs allow overflow TABLEs do not"
date:   2015-09-16 07:00:00
categories:
---

You could say that I have been wanting to write this article for a while from how I [tweeted it](https://twitter.com/getsetbro/status/120936678981509120) in 2011. But the idea deserves a longer explanation than what a tweet provides.

Here is the joke:

<a href="https://getsetbro.github.io/images/divsallowoverflow/cssisawesome.png" style="display:block;text-align:center;padding:15px;">
  <img src="https://getsetbro.github.io/images/divsallowoverflow/cssisawesome.png"/>
</a>

I didn't want to post the picture, but some would not know what I was referring to.

The - CSS is awesome - joke is not what inspired me to write this article today. So stay tuned until the very end to find what I really wanted to pervey.

What I was pointing out in 2011 and what I want to expound on today is that the bug shown in the joke is not a CSS issue. It comes from how DIVs allow overflow where TABLEs never did.
<a href="https://getsetbro.github.io/images/divsallowoverflow/cssisawesomerin3d.png" style="display:block;text-align:center;padding:15px;">
<img src="https://getsetbro.github.io/images/divsallowoverflow/cssisawesomerin3d.png"/>
</a>

I am guessing that this joke began when developers stopped using TABLEs and started using DIVs and found that content could overrun the borders.

This lead them to feel that CSS was indeed NOT AWESOME because it caused problems. But it was the markup all along.

## But what is the real reason for this post?

In some cases you might need to use the TABLE layout to avoid this bug that the block-level-DIV has caused.

Lets say you have a site with a header, main area, and footer. You will probably have some kind of decoration on the header and footer like a background color or borders. Those will go the full width of the page.
<a href="https://getsetbro.github.io/images/divsallowoverflow/divlayout1.png" style="display:block;text-align:center;padding:15px;">
<img src="https://getsetbro.github.io/images/divsallowoverflow/divlayout1.png"/>
</a>

Sometimes for some reason you have to have content that could become wider than the width of the page. And there is nothing you can do about it.

When you scroll horizontally to see the content that is farther to the right you will find that the header and footer do not continue as far as the content.
<a href="https://getsetbro.github.io/images/divsallowoverflow/divlayout2.png" style="display:block;text-align:center;padding:15px;">
<img src="https://getsetbro.github.io/images/divsallowoverflow/divlayout2.png"/>
</a>

### Why did this happen‽

Because the header and footer are block level elements - they are as wide as their parent. The BODY and HTML elements are also block level elements. They are not working like a table. They allow overflow. A table would widen to make sure that it goes all the way around its content - like a mother goose, protecting her young. Even if you set it to be a certain width it would break that rule to hug it's children.

### So what can be done‽

If you want the header and footer to expand as far as the content you could set their parent to act like a TABLE like this:

```
body{
display:table;
}
```

For my example I set it on the BODY element. You can see the [example bin here.](http://jsbin.com/muyowa/edit?html,css,output)
<a href="https://getsetbro.github.io/images/divsallowoverflow/cssisawesomewithscroll.png" style="display:block;text-align:center;padding:15px;">
<img src="https://getsetbro.github.io/images/divsallowoverflow/cssisawesomewithscroll.png"/>
</a>

---

Comments can happen here: [/blog/issues/16](https://github.com/getsetbro/blog/issues/16)
