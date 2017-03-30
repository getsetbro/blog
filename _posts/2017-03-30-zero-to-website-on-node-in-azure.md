---
layout: post
title:  "Zero to website on Node in Azure"
date:   2017-03-30 09:00:00
categories:
---

Steps for going from zero, nothing setup, to a simple site built with node and hosted in azure. This simple site will have one route to the index.html page. The html page will point to a static hosted image file. It will also call an api which pulls data from a database (documentDB but treated like it is mongoDB).

- Download and install the latest stable version of Node from [https://nodejs.org](nodejs.org)
- Create a folder in your documents or desktop, I will name mine 'dev'
- Open a terminal tab to this folder
- Type npm init to initialize with a package.json file. Press ENTER through all the prompts to select the defaults.
- Add Hapi by entering 'npm install Hapi -save'. This installs and adds to the package.
- Create a 'index.js' file in the 'dev' folder. Copy and paste the first example from [https://hapijs.com](https://hapijs.com)
- In the terminal window, that is open to the 'dev' folder type 'node .' This tells Node to run the server.
- In a browser go to 'localhost:8000/hello' to view the app.
- Use 'ctrl C', in the terminal window, to shut it down the server app.
- Next lets change the server connection port to be ready for deployment to azure. 'server.connection({ port: process.env.PORT || 7070 });'
- Add a 'views' folder to the 'dev' folder. Add an 'index.html' html document with and H1 of 'hi'. Be sure to use a web text editor, TextEdit gave me issues at this step.
- npm install vision and handlebars
- change Routes block to this
{% highlight ruby %}
  server.register(require('vision'), function(err) {
      if (err) {throw err;}
      server.route({
          method: 'GET',
          path: '/',
          handler: function (request, reply) {reply.view('index');}
      });
      server.views({
          engines: { html: require('handlebars') },
          relativeTo: __dirname,
          path: 'views',
      });
  });
{% endhighlight %}


---

<!-- Comments can happen here: [/blog/issues/16](https://github.com/getsetbro/blog/issues/16) -->
