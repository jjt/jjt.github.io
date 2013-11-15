---
published: false
title: "Easy Angular development, with Yeoman and generator-angular"
layout: post
---

One of the best things to happen in web development over the past few years has been the variety of tools built on [Node.js][1001] and made available through its package manager, [npm][1000]. Among the 75,000 or so modules, there are three that I use almost every hour of every working day: [Grunt][0], a versatile task runner; [Bower][1], a frontend asset package manager; and [Yeoman][2], a generator framework that automates repetitive scaffolding and dev/deploy tasks, using Grunt and Bower.

This article will focus on using Yeoman and [generator-angular][3] to get a great Angular app development environment going that offers the following features:

- Development server (connect) with livereload
- Unit and integration testing framework (karma)
- Choice between Javascript (default) and Coffeescript
- Project scaffolding (index.html, scripts/style/images directory, etc)
- Generators for controllers, views, directives, etc.
- Sass/Compass and Coffeescript watch & compilation
- Production build task (uglify, css min, static versioning, concat, etc)

If Angular isn't your thing, there are also [dozens of other Yeoman generators][5] that are worth checking out.

## Getting started

If you haven't used Yeoman before, follow [their instructions][4] to get up and running.

First, we'll install the generator and answer some questions about our project. I'm going to go with the defaults for this install. Feel free to ditch Bootstrap, but I'd recommend keeping the default Angular modules unless you're quite familiar with how it works.

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

Here are some relevant directories/files that Yeoman made for us:

```
▾ app/					# Main directory for application
  ▾ scripts/            # Angular js/coffee directory where most of our
    ▸ controllers/ 		#   yeoman-generated modules will go 
    app.js              # Main application file
  ▸ styles/           	# css/scss
  ▸ views/				# Angular views
    index.html        
▸ test/					# Tests (shocking, I know)
  bower.json			# Bower configuration file
  Gruntfile.js			# Grunt configuration file
  package.json			# Node/npm configuration file
```

## Grunt work

Assuming everything went OK, we should be able to start our server with Grunt:

    $ grunt server
    # ... Grunt will start a bunch of tasks 
    Running "watch" task
    Waiting...

At this point, we have our development server running and Grunt is monitoring our project for changes. If we modify a `.scss` file or `.coffee` file, Grunt will rebuild our css or js, respectively, and livereload will refresh the browser automatically.

Speaking of the browser, running `grunt server` should have opened up a browser tab pointed to `http://127.0.0.1:9000` which will look something like this:

![](/media/ng-generator-1.png)

## Let it all Ang out

So far we've got a basic app running - very basic.





[1000]: https://npmjs.org/
[1001]: http://nodejs.org/
[0]: http://gruntjs.com/
[1]: http://bower.io/
[2]: http://yeoman.io/
[3]: https://github.com/yeoman/generator-angular
[4]: http://yeoman.io/gettingstarted.html
[5]: http://yeoman.io/community-generators.html