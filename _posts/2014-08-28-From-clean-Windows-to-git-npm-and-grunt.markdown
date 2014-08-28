---
layout: post
title:  "From clean Windows to git, npm, and grunt"
date:   2014-08-28 13:34:54
---
Steps for building git stuff on windows - As Easy As Possible

Install git from http://git-scm.com/downloads
During the install choose all defaults except for Run Git from the Windows Command Prompt:

(if you instead install github-for-windows it does not add GIT commands to windows command prompt)

Next Install node from nodejs.org/download
Install 32 or 64 bit.
The default choices install everything needed. NPM and NODE commands are added to windows command prompt (puts command in your system path, allowing it to be run from any directory.)

Install grunt.
From a command prompt type:
npm install -g grunt-cli
This installs grunt and the GRUNT command is added to win command prompt

Now we can git projects and build them. From the command prompt type:
{% highlight ruby %}
git clone https://github.com/angular-ui/bootstrap.git

cd bootstrap

npm install

git checkout origin/bootstrap3_bis2

grunt
{% endhighlight %}

When I tried this I got a windows firewall blocked notification - so i allowed it.

Then I got a couple errors so instead I had to type:
{% highlight ruby %}
grunt --force
{% endhighlight %}

And got: Error launcher could not find chrome "set env variable CHROME_BIN".
No biggy, it built the file needed. Boom, roasted.
