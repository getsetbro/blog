---
layout: post
title:  "Bookmarklet to make your browser a Text Editor"
date:   2015-05-26 12:00:00
categories:
---

A link/bookmarklet that opens a browser window/tab with a big content-editable region so you can start typing some notes, thoughts, ideas, quotes... you get it.

<a title="Open as Text Editor" target="_blank" href="data:text/html;charset=utf-8,%20%3ctitle%3eTextEditor%3c/title%3e%3cbody%20contenteditable%20style=%22font-size:2rem;font-family:monaco;line-height:1.4;max-width:60rem;margin:0%20auto;padding:4rem;%22%20spellcheck=%22false%22%3e%3ch1%3eText%20Editor%3c/h1%3e%3cp%3eStart%20Here.">Open my browser as a Text Editor</a>

Here is where it started, as far as I know <a href="https://coderwall.com/p/lhsrcq" target="_blank">LINK.</a> If you read the comments you can find a bunch of variations. Some that will save your typing to localStorage so closing the window does not loose your stuff.

Here is the code in the link I pasted above.

<pre><code>data:text/html;charset=utf-8,&lt;title&gt;TextEditor&lt;/title&gt;&lt;body%20contenteditable%20style="font-size:2rem;font-family:monaco;line-height:1.4;max-width:60rem;margin:0%20auto;padding:4rem;"%20spellcheck="false"&gt;&lt;h1&gt;Text%20Editor&lt;/h1&gt;&lt;p&gt;Start%20Here.</code></pre>

## Update

Newer hotterness

<pre><code>data:text/html;charset=utf-8,&lt;title&gt;TextArea&lt;/title&gt;&lt;style&gt;html{height: 100%;}body{margin:0;padding:4%;border:20px solid #eee;background-color:#fff;border-radius:2em}textarea{width:100%;height:100%;border:0 none;font-family:&quot;Lucida Grande&quot;,sans-serif;font-size:2em;outline:none;color:#444}&lt;/style&gt;&lt;body&gt;&lt;textarea spellcheck=&quot;false&quot; autofocus=&apos;autofocus&apos;&gt;&lt;/textarea&gt;&lt;/body&gt;&lt;/html&gt;</code></pre>

## Update

Newer hotterness with localStorage

<pre><code>javascript:&apos;&lt;title&gt;TextArea&lt;/title&gt;&lt;style&gt;body{margin:-10px;padding:20px;border:20px solid #aaa;border-radius:1em;font-family:-apple-system,&quot;Roboto&quot;,BlinkMacSystemFont,&quot;Segoe UI&quot;,&quot;Helvetica Neue&quot;,sans-serif;font-size:2em;color:#444}
&lt;/style&gt;&lt;body contenteditable oninput=localStorage.c=innerHTML&gt;&apos;+[localStorage.c]</code></pre>

Just paste the above code into your bookmark.

---

Comments can happen here: [/blog/issues/4](https://github.com/getsetbro/blog/issues/4)
