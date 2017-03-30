---
layout: post
title:  "Zero to website on Node in Azure"
date:   2017-03-30 09:00:00
categories:
---

# Go from having no previous setup to deploying a simple Node website

These are a set of steps for going from having nothing setup to a simple site built with node and hosted in the cloud. This simple site will have one route to the index.html page. The html page will point to a static hosted image file. It will also call an api which pulls data from a database (documentDB but treated like it is mongoDB).

## Prep / Setup

### Azure web app

- In Azure create a new Web App. You will give it a name, select a Resource Group, and select a Service Plan.
- When that is created click the Quickstart button in the Web App properties. Choose Node and then 'Local Source Control'.
- Click the link to configure credentials. Make a Username and Password.
- Click to the Deployment Options. Choose 'Local Git Repo'. Click OK.
- Find the GIT URL on the properties page of your Web App.

### Git

- Download and install git from [https://git-scm.com/](https://git-scm.com/)
- Open your Command Line app to a folder where you want to work on the repo.
- Type 'git clone' plus space, and then the path your repo. All together it will look like: `git clone https://USERNAME@WEB-APP-NAME.scm.azurewebsites.net:443/WEB-APP-NAME.git`.
- It will download it and may report back that it is empty.

### Node and Hapi

- Download and install the latest stable version of Node from [https://nodejs.org](https://nodejs.org).
- Make sure your Command Line app is still open to the repo folder.
- Type `npm init` to initialize with a 'package.json' file. Press ENTER through all the prompts to select the defaults.
- Add Hapi by entering `npm i Hapi -S`. The 'i' installs and the '-S' adds it to the package as a dependency for this app. This will tell Azure what packages are needed to build this project.

## Create the server with Hapi

- Create a 'index.js' file in the repo folder. This can also be 'server.js' or 'app.js'.
- Copy and paste into it the first example from [https://hapijs.com/](https://hapijs.com/)
- In the terminal window, that is open to the 'dev' folder type `node .` or `npm start`. This tells Node to run the server.
- In a browser go to 'localhost:8000/hello' to view the app.
- Use `ctrl C`, in the terminal window, to shut down the app.
- Next lets change the server connection port to be ready for deployment to a cloud service. I am dropping localhost, if that breaks things for you than put it back.
  ```javascript
  server.connection({ port: process.env.PORT || 7070 });
  ```

## Serving html

- Add a 'views' folder to the 'dev' folder.
- Add an 'index.html' file with HTML code such as:
  `<!doctype html><html><head><meta charset="UTF-8"></head><body>About</body></html>`
  Be sure to use a web text editor, TextEdit gave me issues at this step.
- With NPM install the node modules 'vision' and 'handlebars': `npm i -S vision` and `npm i -S handlebars`
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

## Serve up static files

- Create a folder named 'public'.
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

- In Azure Click NEW > DATABASES > NOSQL/DOCUMENTDB.
- Give it a name.
- Click to give it a MongDB API.
- Choose a Resource Group and Location.
- Click Ok.
- In the properties click the Quickstart button. Choose the Node.js tab and find the connection string.
- In the 'index.js' file add a new block
```javascript
const MongoDB = require('hapi-mongodb');
const dbOpts = {
    url: 'PASTE-DB-CONNECTION-STRING-HERE',
    settings: {
        db: { native_parser: false },
    },
};
server.register({ register: MongoDB, options: dbOpts }, err => {
    server.route([
        {
            method: 'GET',
            path: '/allbooks',
            config: {
                handler: (request, reply) => {
                    var db = request.server.plugins['hapi-mongodb'].db;
                    reply(db.collection('booksDBColl').find({}, { title: 1 }).toArray());
                },
                cors: true
            },
        },
        {
            method: 'POST',
            path: '/addbook',
            config: {
                handler: (request, reply) => {
                    var dbDoc = { title: request.payload.title };
                    var db = request.server.plugins['hapi-mongodb'].db;
                    db.collection('booksDBColl').updateOne({ title: request.payload.title }, dbDoc, { upsert: true }, (err, result) => {
                        return reply(result);
                    });
                },
                cors: true
            }
        }
    ]);
});
```
- In your index file paste this
```
<h1>books</h1>
<ul id="ul"></ul>
<p>add: <input type="text" id='input' />
    <button type="button" id='btn'>add</button>
</p>
<script src='https://code.jquery.com/jquery-latest.js'>
    //v1.11.1
</script>
<script>
    var getr = $.ajax({
        url: "/allbooks",
        method: "GET"
    });
    getr.then(function (d) {
        $.each(d, function (k, v) {
            $('#ul').append('<li>' + v.title + '</li>');
        });
    });

    $('#btn').on('click', function (e) {
        var inpt = $('#input').val();
        var sendr = $.ajax({
            url: "/addbook",
            method: "POST",
            data: { title: inpt }
        });

        var getr2 = $.ajax({
            url: "/allbooks",
            method: "GET"
        });
        getr2.then(function (d) {
            $('#ul').html('');
            $.each(d, function (k, v) {
                $('#ul').append('<li>' + v.title + '</li>');
            });
        });
    })
</script>
```

- Test

---

Comments can happen here: [/blog/issues/17](https://github.com/getsetbro/blog/issues/17)
