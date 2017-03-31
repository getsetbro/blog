---
layout: post
title:  "Zero to website on Node in Azure"
date:   2017-03-30 09:00:00
categories:
---

## Go from having no previous setup to deploying a simple Node website

These are a set of steps for going from having nothing setup to a simple site built with node and hosted in the cloud. This simple site will have one route to the index.html page. The html page will point to a static hosted image file. It will also call an API which pulls data from a database ( DocumentDB but with the MongoDB API ).

## Prep / Setup

### Node, NPM, and Hapi

- Install the latest stable version of Node from [https://nodejs.org](https://nodejs.org).
- Create a folder named 'dev' in your Documents folder.
- Open your command-line tool to the 'dev' folder.
- Type `npm init` to initialize by making a 'package.json' file. Press ENTER through all the prompts to select the defaults.
- Add Hapi by entering `npm install --save Hapi`. The '--save' (or '-S') adds it to the 'package.json' file as a dependency for this app. This will tell cloud service what packages are needed to build this project.

## Create the app

### Hapi

Competitors to Hapi are Express, Koa, Lazo, Restify - I am keeping a list [here](https://github.com/getsetbro/csv-files/blob/master/node-tools.csv)

- Create a 'index.js' file in the 'dev' folder. This can also be 'server.js' or 'app.js'.
- Copy and paste into 'index.js' the first example from [https://hapijs.com/](https://hapijs.com/)
- In the terminal window, that is open to the 'dev' folder type `node .`. This tells Node to run the app.
- In a browser go to 'http://localhost:8000/hello' to view the app.
- Use `ctrl C`, in the terminal window, to shut down the app.
- Next lets change the server connection port to be ready for deployment to a cloud service. I am dropping localhost, if that breaks things for you than put it back.
  ```javascript
  server.connection({ port: process.env.PORT || 7070 });
  ```

### Serving HTML and static files

- Add a 'views' folder to the 'dev' folder.
- Add an 'index.html' file with HTML code such as:
    ```
    <!doctype html>
    <html>
    <head><meta charset="UTF-8"></head>
    <body>Hello</body>
    </html>
    ```
- Be sure to use a web text editor, TextEdit gave me issues at this step.
- With NPM install the node modules 'vision' and 'handlebars': `npm install -S vision && npm install -S handlebars`
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

- Now start the app from the terminal window with `node .`.
- Check it out at 'http://localhost:7070/'. You should see 'Hello' from your index view.

### Serve up static files

- Create a folder named 'public'.
- Add an image file named 'logo.png' or 'logo.jpg' just remember which one.
- With NPM install inert: `npm install -S inert`
- In the 'index.js' file add the following block above this line `server.register(require('vision')...`:
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

- In the BODY of the 'index.html' file add the logo image `<img src="/img/logo.png" />` or  `<img src="/img/logo.jpg" />`.
- Now start the app from the command-line tool with `node .`.
- Check it out at 'http://localhost:7070/'. You should see the image in the web page.

## Deploying to a cloud service

In this demo we will be deploying to Azure but it could any PaaS. I am keeping a list [here](https://github.com/getsetbro/csv-files/blob/master/node-tools.csv)

### Azure

- In the Azure portal create a NEW Web App. Give it a name, a Resource Group, and a Service Plan.
- When it is created click the Quickstart button in the Web App properties. Choose Node and then 'Local Source Control'.
- Click the link to configure credentials. Make a Username and Password.
- Click to the Deployment Options. Choose 'Local Git Repo'. Click OK.
- Find the GIT URL on the properties page of your Web App.

### Git

- Install Git from [https://git-scm.com/](https://git-scm.com/) or see all the ways [here](https://www.atlassian.com/git/tutorials/install-git)
- In command-line tool type `git init` to initialize this folder as a git repo.
- Add a .gitignore file to your repo paste these two items into it:
```
node_modules
.DS_Store
```
- Type `git add -A` to add all files to the git repo.
- Type `git commit -m "Hello Azure App Service"` to commit the changes.
- Type `git remote add azure https://USERNAME@WEBAPPNAME.scm.azurewebsites.net:443/WEBAPPNAME.git`
- Type `git push azure master`

The official MS Docs with more info can be found [here](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-deploy-local-git)

## Add a Database

### DocumentDB but really MongoDB but really DocumentDB

- In the Azure portal Click NEW > DATABASES > NOSQL/DOCUMENTDB.
- Give it a name.
- Click to make it use the MongDB API.
- Choose a Resource Group and Location.
- Click Ok.
- In the properties click the Quickstart button. Choose the Node.js tab and find the connection string.

### Create an API with I/O to the database

- First install the 'hapi-mongodb' package with `npm install -S hapi-mongodb`.
- In the 'index.js' find the block that starts with `server.start(err => {...`
- Instead of that whole block paste this one but include your Database Connection String:

    ```javascript
    var MongoDB = require('hapi-mongodb');
    var dbOpts = {
        url: 'DB_CONNECTION_STRING_HERE',
        settings: {
            db: { native_parser: false },
        },
    };
    server.route([
        {
            method: 'GET',
            path: '/allbooks',
            config: {
                handler: function(request, reply){
                    var db = request.server.plugins['hapi-mongodb'].db;
                    reply(db.collection('books').find({}, { title: 1 }).toArray());
                },
                cors: true
            },
        },
        {
            method: 'POST',
            path: '/addbook',
            config: {
                handler: function(request, reply){
                    var db = request.server.plugins['hapi-mongodb'].db;
                    db.collection('books').updateOne(
                    { title: request.payload.title },
                    { title: request.payload.title },
                    { upsert: true },
                    function(err, result){
                        return reply(result);
                    }
                    );
                },
                cors: true
            }
        }
    ]);
    server.register({ register: MongoDB, options: dbOpts }, function(err){
        if (err) { throw err; }
        server.start(function(err){
            console.log(server.info.uri);
        });
    });
    ```

- Let's walk through what the above code does.
- - It 'requires' the hapi-mongodb package.
- - It includes the DB connection string from azure
- - It creates two routes. One for getting all records from the database and one for adding records to the database.
- - Then it starts the server after registering the MongoDB module.

### Use the API from the view

- In the 'index.html' file add this jQuery script under the IMG tag: `<script src='https://code.jquery.com/jquery-3.2.1.slim.min.js'></script>`
- Then add this block inside of a new SCRIPT block:
```javascript
var sendr = $.ajax({url:"/addbook", method:"POST", data:{ title: "Title " + new Date().getTime() }});
sendr.then(function(){
  $.ajax({url:"/allbooks", method:"GET"}).then(function (d) {
    $.each(d, function (k, v) {$(document.body).append('<div>' + v.title + '</div>');});
  });
});
```
- The above code sends off a request tells the API to create a new record in the database. When that request is complete it asks the api to get all records and appends them to the BODY.

---

Comments can happen here: [/blog/issues/17](https://github.com/getsetbro/blog/issues/17)
