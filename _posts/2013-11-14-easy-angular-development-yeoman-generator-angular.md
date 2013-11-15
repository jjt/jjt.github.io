---
published: false
title: "Easy Angular development, with Yeoman and generator-angular"
layout: post
---

One of the best things to happen in web development over the past few years has been the variety of tools built on [Node.js][1001] and made available through its package manager, [npm][1000]. Three of my most-used command line tools are [Grunt][0], a versatile task runner; [Bower][1], a frontend asset package manager; and [Yeoman][2], a generator framework that automates repetitive scaffolding and dev/deploy tasks, using Grunt and Bower.

This article will focus on using the Yeoman and [generator-angular][3] to get a great baseline Angular app development environment going. If Angular isn't your thing, there are also have [dozens of other generators][5] that are worth checking out. If you haven't used Yeoman before, follow [their instructions][4] to get up and running.


## Getting started

After the initial setup, we'll have a great Angular development environment with the following features:

- Development server (connect) with livereload
- Unit and integration testing framework (karma)
- Choice between Javascript (default) and Coffeescript
- Project scaffolding (index.html, scripts/style/images directory, etc)
- Generators for controllers, views, directives, etc. 
- Sass/Compass and Coffeescript watch & compilation
- Production build task (uglify, css min, static versioning, concat, etc)

First, we'll install the generator and answer some questions about our project. I'm going to go with the defaults for this install. Feel free to ditch Bootstrap, but I'd recommend keeping the modules unless you're quite familiar with how Angular works.

```shell
$ mkdir myApp && cd myApp
$ npm install -g generator-angular
$ npm install generator-angular
$ yo angular myApp (--coffee)

[?] Would you like to include Twitter Bootstrap? (Y/n)
[?] Would you like to use the SCSS version of Twitter Bootstrap with the Compass CSS Authoring Framework? (Y/n)
[?] Which modules would you like to include? (Press <space> to select)
❯⬢ angular-resource.js
 ⬢ angular-cookies.js
 ⬢ angular-sanitize.js
 ⬢ angular-route.js
```

You'll now see approximately 20 metric assloads of output to the screen while Yeoman scaffolds out our project and pulls down npm and Bower modules. If that finishes okay, we'll have our development environment all set up!

## 

Assuming everything went OK, we should be able to start our server with Grunt:

    $ grunt server
    
    Running "server" task
    # ... 
    Running "watch" task
    Waiting...

At this point, we have our development server running and Grunt is monitoring our project for changes. If we modify a `.scss` file or `.coffee` file, Grunt will recompile the Sass/Coffeescript respectively and livereload will refresh the browser automatically.

Speaking of the browser, running `grunt server` should have opened up a browser tab with our app, which will look something like this:

![Screenshot of Angular app on first run](/media/ng-generator-1.png)

## Let's do some stuff

```
▾ app/					# Main directory for application
  ▸ bower_components/   
  ▾ scripts/            # Angular js/coffee directory where most of our
    ▾ controllers/ 		#   yo-generated modules will go
        main.js
      app.js
  ▾ styles/           	# css/scss
      main.scss
  ▾ views/				# Angular views
      main.html
    404.html
    favicon.ico
    index.html
    robots.txt
▸ node_modules/
▾ test/
  ▾ spec/
    ▾ controllers/
        main.js
    runner.html
  bower.json
  Gruntfile.js
  karma-e2e.conf.js
  karma.conf.js
  package.json
```



[1000]: https://npmjs.org/
[1001]: http://nodejs.org/
[0]: http://gruntjs.com/
[1]: http://bower.io/
[2]: http://yeoman.io/
[3]: https://github.com/yeoman/generator-angular
[4]: http://yeoman.io/gettingstarted.html
[5]: http://yeoman.io/community-generators.html