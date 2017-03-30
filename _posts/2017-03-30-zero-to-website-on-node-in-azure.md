---
layout: post
title:  "Zero to website on Node in Azure"
date:   2017-03-30 09:00:00
categories:
---

These are steps for going from zero, nothing setup, to a simple site built with node and hosted in azure. This simple site will have one route to the index.html page. The html page will point to a static hosted image file. It will also call an api which pulls data from a database (documentDB but treated like it is mongoDB).

### Prep / Setup

- Download and install the latest stable version of Node from [https://nodejs.org](https://nodejs.org).
- Create a folder on your system, let's name it 'dev'.
- Open a terminal tab to this folder.
- Type npm init to initialize with a package.json file. Press ENTER through all the prompts to select the defaults.
- Add Hapi by entering 'npm i Hapi -S'. The 'i' installs and the '-S' adds it to the package as a dependency for this app. This will tell the cloud service what packages are needed to build this project.

### Create the server with Hapi

- Create a 'index.js' file in the 'dev' folder.
- Copy and paste the first example from [https://hapijs.com/](https://hapijs.com/)
- In the terminal window, that is open to the 'dev' folder type `node .` or `npm start`. This tells Node to run the server.
- In a browser go to 'localhost:8000/hello' to view the app.
- Use `ctrl C`, in the terminal window, to shut down the app.
- Next lets change the server connection port to be ready for deployment to a cloud service. I am dropping localhost, if that breaks things for you than put it back.
  ```javascript
  server.connection({ port: process.env.PORT || 7070 });
  ```

### Serving html

- Add a 'views' folder to the 'dev' folder.
- Add an 'index.html' file with HTML code such as:
  ```
  <!doctype html>
  <html>
  <head>
      <meta charset='UTF-8'>
      <title>doc</title>
  </head>
  <body>About</body>
  </html>
  ```

- Be sure to use a web text editor, TextEdit gave me issues at this step.
- With NPM install vision and handlebars:
  `npm i -S vision` and `npm i -S handlebars`
- Change the 'Routes' block to this:
  ```javascript
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
  ```

- Now start the app from the terminal window with `node .` or `npm start`.
- Check it out at http://localhost:7070/. You should see the text in the BODY of your index file.

### Serve up static files

- Create a folder in 'dev' named 'public'.
- Add an image file named 'logo.png' or 'logo.jpg' just remember which one.
- With NPM install inert:
  `npm i -S inert`
- In the 'index.js' file add this block above `server.register(require('vision')...`
```javascript
server.register(require('inert'), function(err){
    if (err) {throw err;}
    server.route({
        method: 'GET',
        path: '/{file*}',
        handler: {
            directory: {path: 'public'},
        },
    });
});
```

- In the 'index.html' file (in the views folder) add the logo image `<img src="/img/logo.png" />` or  `<img src="/img/logo.jpg" />` in the BODY.
- Now start the app from the terminal window with `node .` or `npm start`.
- Check it out at http://localhost:7070/. You should see the image in the web page.

### Create an API for I/O with a database

-

---

Comments can happen here: [/blog/issues/17](https://github.com/getsetbro/blog/issues/17)
