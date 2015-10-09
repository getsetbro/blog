---
layout: post
title:  "Prototype Driven Development"
date:   2015-03-02 13:00:00
categories:
---

Prototype Driven Development provides quick testing of UI components.

Remember how Sketch Flow was made in a way that the effort put into creating wireframes for a SilverLight project would not be throw-away work? No? Well the concept was great. Let's do that with Prototypes for web app development.

Prototypes are more valuable than wireframes or designs. You will see that requirements change once clients can touch what it is that they have requested you build for them. "Oh it responds like that? Can you just make that different?"

---

**Contents**

* Table of Contents Placeholder
{:toc}

---

###Prototyping

A prototype will provide proof of concepts and be a source of documentation throughout the project.

Proper setup of build tools means that the code in the prototype will be the same code in production.

Example: [PatternLab](//demo.patternlab.io). Shows the single components, components in groups, and shows the code used to build them. It shows the components alone as well as next to other components. It shows if a component is ready for re-use.

<a href="//demo.patternlab.io/?p=molecules-comment-form" target="_blank">single-form</a> |
<a href="//demo.patternlab.io/?p=organisms-comment-thread" target="_blank">comment-thread</a>

<iframe src="http://demo.patternlab.io/?p=molecules-comment-form" style="border: 1px solid rgb(170, 170, 170); width: 100%; min-height: 300px;"></iframe>

---

###Build System

With an enhanced build system we can build the Prototype + Style Guide alongside our project in VS.

**Installs**

- **Git**. Needed to "Git" things from the web. - [//msysgit.github.io](//msysgit.github.io)
- **Node.js**. [//nodejs.org](//nodejs.org)
  - Once installed you can "Git" other things with npm:
  - No Ruby, PHP, or Java needed
- **Gulp or Grunt** or both: "Automation, performing repetitive tasks"
  - minification, concatenation...
- **Pattern Lab** (node version)
  - Builds a Prototype as well as a [Style Guide](//demo.patternlab.io/styleguide/html/styleguide.html)!
- **Optional** add-ons for VS (works on VS 2013):
  - [Task Runner Explorer](//visualstudiogallery.msdn.microsoft.com/8e1b4368-4afb-467a-bc13-9650572db708) - See tasks in a VS explorer window.
  - [Package Intellisense](//visualstudiogallery.msdn.microsoft.com/65748cdb-4087-497e-a394-2e3449c8e61e) NPM and Bower packages directly with Intellisense.
  - [Grunt Launcher](//visualstudiogallery.msdn.microsoft.com/dcbc5325-79ef-4b72-960e-0a51ee33a0ff) - Launch Grunt, Gulp and Bower commands in Visual Studio.

Why not Nuget and MSBuild? [Hanselman answered](//www.hanselman.com/blog/IntroducingGulpGruntBowerAndNpmSupportForVisualStudio.aspx). "Because there's already a rich ecosystem for this kind of thing. NuGet is great for server side libraries (and some client-side) but there are so many more CSS and JS libs on npm and bower. MSBuild is great for server-side builds but can be overkill when building a client-side app."

---

###JavaScript

We'll need some JS Patterns to avoid conflicts so components can be reusable. If two components want to save a number to a variable named "width" then the last one to define it will win. And the app will lose.

**Modular Pattern**
[//jsbin.com/lumeqa/12/edit?html,js,output](//jsbin.com/lumeqa/7/edit?html,js,output)

<iframe src="//jsbin.com/lumeqa/12/embed?js" class="" id="" style="border: 1px solid rgb(170, 170, 170); width: 100%; min-height: 300px;"></iframe>

<br>

Or just use the jQuery plug-in format.
[//jsbin.com/lumeqa/14/edit?html,js,output](//jsbin.com/lumeqa/8/edit?html,js,output)

<iframe src="//jsbin.com/lumeqa/14/embed?js" class="" id="" style="border: 1px solid rgb(170, 170, 170); width: 100%; min-height: 300px;"></iframe>

<br>

Elements affected by JS should have separate css classes that look different: "js-icon" or potentially data-attributes. That way you can safely change style classes but carefully change JS classes.

Example(s)

- &lt; i class="icon-class js-icon" data-icon="iconJS" &gt;
- $(".js-icon").on(...);
- $("[data-icon]").on(...);

<br>

<iframe src="//jsbin.com/lumeqa/11/embed?html,js" class="" id="" style="border: 1px solid rgb(170, 170, 170); width: 100%; min-height: 300px;"></iframe>
<br>

---

###CSS

As a base use the latest bootstrap. Everybody learn bootstrap. They have put much time into finding the configuration that works best for the web today. You can transfer your knowledge to other UI frameworks later.

If you want different margins then customize bootstrap before you download it. Only build something new if bootstrap does not provide it - it usually does provide it. Then when we do need to create something new use conventions. Building without a having reuse in mind is a shot in the foot.

**Conventions**

SUIT CSS is a reliable and testable styling methodology for component-based UI development. The SUIT CSS naming convention helps to scope component CSS and make your widgets render more predictably.

+ **.ComponentName** "Article"
+ **.ComponentName--modifierName** "Article--black"
+ **.ComponentName-descendentName** "Article-title"
+ **.u-utilityName** "u-floatLeft"
+ **.is-stateName** "is-selected"

"The component's name must be written in pascal case. Nothing else in the HTML/CSS uses pascal case." <a href="//github.com/suitcss/suit/blob/master/doc/naming-conventions.md" target="_blank">SUIT CSS naming-conventions</a>

For a full demo see: [//jsbin.com/lumeqa/2/](//jsbin.com/lumeqa/2/)

<iframe src="//jsbin.com/lumeqa/10/embed?html" class="" id="" style="border: 1px solid rgb(170, 170, 170); width: 100%; min-height: 300px;"></iframe>

<br>
It is a learning curve for sure but the results are worth it. This will scale from small to large projects and make for happier teams.

---

###SVG & Icon font opinions

No more IMG for icons.

Font icons are easy to use. Their icons pick up the font color. I very really like icomoon [//icomoon.io](//icomoon.io)

But SVG wins the cagematch: [//css-tricks.com/icon-fonts-vs-svg](//css-tricks.com/icon-fonts-vs-svg) Automated grunt task. SVG can be animated. [//grunticon.com](//grunticon.com)

All browsers since IE9 do SVG so it's time to adopt. [//caniuse.com/#feat=svg](//caniuse.com/#feat=svg)

What does SVG look like in my code? [//jsbin.com/nufuqi/2/edit](//jsbin.com/nufuqi/1/edit)

<iframe src="//jsbin.com/nufuqi/2/embed?html,output" class="" id="" style="border: 1px solid rgb(170, 170, 170); width: 100%; min-height: 300px;"></iframe>

Since you do not like that all of that SVG code move the SVG code to a re-usable block and pull it in with the USE element [//jsbin.com/nufuqi/3/edit](//jsbin.com/nufuqi/1/edit)

<iframe src="//jsbin.com/nufuqi/3/embed?html,output" class="" id="" style="border: 1px solid rgb(170, 170, 170); width: 100%; min-height: 300px;"></iframe>

SVG sprites so the code stays clean and repurposable: [//css-tricks.com/svg-sprites-use-better-icon-fonts](//css-tricks.com/svg-sprites-use-better-icon-fonts) and [//css-tricks.com/svg-symbol-good-choice-icons/](//css-tricks.com/svg-symbol-good-choice-icons/)

How might I make a shape? [//blog.getsetbro.com/js/the-svg-tutorial-that-will-make-it-click.html](//blog.getsetbro.com/js/the-svg-tutorial-that-will-make-it-click.html)

---

###PS

A thought about the teams who build this. Often a "front-end" team builds the prototype and hands it off for it to be integrated into production by the "back-end" team.
This might lead to the second team finding that the components have bugs after integration that were not there in the prototype. At this point either team can make necessary changes with both integration and the prototype in mind.

---

Comments can happen here: [/blog/issues/7](https://github.com/getsetbro/blog/issues/7)