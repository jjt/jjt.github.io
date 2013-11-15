---
published: false
title: "Easy Angular development with Yeoman and generator-angular"
layout: post
---

One of the best things to happen in web development over the past few years has been the variety of tools built on [Node.js][1001] and made available through its package manager, [npm][1000]. Among the 75,000 or so modules, there are three that I use almost every hour of every working day: [Grunt][0], a versatile task runner; [Bower][1], a frontend asset package manager; and [Yeoman][2], a generator framework that automates repetitive scaffolding and dev/deploy tasks, using Grunt and Bower.

This article will focus on using Yeoman and [generator-angular][3] to set up a great Angular app development environment with the following features: 

- Development server (connect) with livereload
- Unit and integration testing framework (karma)
- Choice between Javascript (default) and Coffeescript
- Project scaffolding (index.html, scripts/style/images directories, etc)
- Generators for controllers, views, directives, etc.
- Sass/Compass and Coffeescript watch & compilation
- Production build task (uglify, css min, static versioning, concat, etc)

If Angular isn't your thing, there are also [dozens of other Yeoman generators][5] that are worth checking out.

## Getting started

If you haven't used Yeoman before, follow [their instructions][4] to get up and running.

First, we'll install the generator and answer some questions about our project. I'm going to go with the defaults for this install. Feel free to ditch Bootstrap, but I'd recommend keeping the default Angular modules unless you have a compelling reason to strip them out.

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

We'll now see approximately 20 metric assloads of output to the screen while Yeoman takes a minute to scaffold out our project and pull down npm and Bower modules. If that finished okay, we'll have our development environment all set up.

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

At this point, we have our development server running and Grunt is monitoring our project for changes. If one of our tests change (or we touch the file), Grunt will run it. If we modify a `.scss` file or `.coffee` file, Grunt will rebuild our css or js, respectively, and livereload will refresh the browser automatically.

Speaking of the browser, running `grunt server` should have opened up a browser tab pointed to `http://127.0.0.1:9000` which should be the front page of our app with a big, green "Splendid!" button.

## Let it all Ang out

The focus of this post is the Yeoman generator so some Angular knowledge is assumed for this section. As long as you're familiar with MVC frameworks you should be able to follow along. If you're looking for more how-to-Angular, I recommend the classic [Egghead.io videos][6].

So far we've got an enthusiastic greeting from Yeoman, but we'll probably want to add our own routes and controllers. Yeoman has us covered.

When we ran `yo angular myApp`, we were using a synonym of `angular:app`, which is one generator out of a dozen or so available to us. We can get the list of available generators by typing `yo --help`, but here's a better reference:

- [angular](https://github.com/yeoman/generator-angular#app) (aka [angular:app](https://github.com/yeoman/generator-angular#app))
- [angular:controller](https://github.com/yeoman/generator-angular#controller)
- [angular:directive](https://github.com/yeoman/generator-angular#directive)
- [angular:filter](https://github.com/yeoman/generator-angular#filter)
- [angular:route](https://github.com/yeoman/generator-angular#route)
- [angular:service](https://github.com/yeoman/generator-angular#service)
- [angular:provider](https://github.com/yeoman/generator-angular#service)
- [angular:factory](https://github.com/yeoman/generator-angular#service)
- [angular:value](https://github.com/yeoman/generator-angular#service)
- [angular:constant](https://github.com/yeoman/generator-angular#service)
- [angular:decorator](https://github.com/yeoman/generator-angular#decorator)
- [angular:view](https://github.com/yeoman/generator-angular#view)

We're going to use a few of them now to create a new `foo` route, with a matching controller and view. Assuming you want all three to be named `foo`, you can take a shortcut by just using the route generator, which will call the controller and view  generators in turn.

```shell
$ yo angular:route foo
   invoke   angular:controller:/path/to/myApp/node_modules/generator-angular/route/index.js
   create     app/scripts/controllers/foo.js
   create     test/spec/controllers/foo.js
   invoke   angular:view:/path/to/myApp/node_modules/generator-angular/route/index.js
   create     app/views/foo.html
```
So what did this do for us?

- Created a `foo` controller with a matching test
- Created a `foo` view
- Added a `foo` route in the main `app.js` file, lacing up the controller and view
- Added `<script>` tags to `index.html`

If we were to go to `http://127.0.0.1:9000/#/foo` now, we would see the stunningly beautiful sight of "This is the foo view" on a blank page. Now is when we'd crack open the files we just created and make our foo page start to come to life. That is beyond the scope of this article, however.

The other generators are much the same in their functionality - they exist to save  time by handling all the monotonous scaffolding tasks.

## Smash the hash: using html5mode for pretty urls

You might have noticed that we used a hash in our url routing. This is the way that Angular ships by default. Fortunately for us more picky devs, there exists the [html5mode][7] setting which uses htmlPushState with a fallback shim for unsupported browsers.

Everything will work great when clicking around in our app, but it will 404 on refresh because the connect dev server doesn't know how to serve anything other than `/` and static files. Sounds like a job for `connect-modrewrite`! First we'll install it, saving it to our package.json:

    $ npm install --save-dev connect-modrewrite

And then we'll add the following snippet to our Gruntfile in the `connect.options` object in the `grunt.initConfig` function call. It will rewrite anything other than a static file to `index.html`.

```js
connect: {
  options: {
    // ...
    // Modrewrite rule, connect.static(path) for each path in target's base
    middleware: function (connect, options) {
      var optBase = (typeof options.base === 'string') ? [options.base] : options.base;
      return [require('connect-modrewrite')(['!(\\..+)$ / [L]'])].concat(
        optBase.map(function(path){ return connect.static(path); }));
    }
  }
}
``` 

So we reload our browser at `http://127.0.0.1:9000/foo` and it loads, but there's nothing happening on the screen and there's no css. That's because generator-angular assumes that we'll be using the hash mode and generates relative paths like:

    <script src="scripts/app.js"></script>
    
My low tech solution is just going into `index.html` and turning every script and link tag into a root-relative one:

    <script src="/scripts/app.js"></script>

Another option to the relative urls is to set the base url of the site by putting `<base href="/">` in the head, but that's not without [its problems][8]. There is [some discussion][9] on the generator-angular issue tracker about adding html5mode support, or at least adding the option of a leading slash on the script/link tags. 

Note that enabling html5mode support also requires us to have a production server that can do the modrewriting. One of the advantages of Angular is that we can ordinarily do a static deploy to S3 or another static server, reducing devops work and server cost. So it's up to you to decide whether or not html5mode is worth it.

## Fly my pretties!

Once we've developed our app and you want to deploy a release, we can do so with the `grunt build` command. It will run a whole bunch of handy tasks, like minify/uglify, jshint, ngmin, concatenation, etc. Once it's successfully done, it will result in a production-ready build under the `dist` directory. We can now push the contents of that directory to a static server and baby, we got a stew goin!

I hope this article has shown how Yeoman and generator-angular can help you to more quickly and efficiently develop Angular applications.


[1000]: https://npmjs.org/
[1001]: http://nodejs.org/
[0]: http://gruntjs.com/
[1]: http://bower.io/
[2]: http://yeoman.io/
[3]: https://github.com/yeoman/generator-angular
[4]: http://yeoman.io/gettingstarted.html
[5]: http://yeoman.io/community-generators.html
[6]: http://egghead.io/lessons
[7]: http://docs.angularjs.org/guide/dev_guide.services.$location#general-overview-of-the-api_$location-service-configuration
[8]: http://stackoverflow.com/questions/1889076/is-it-recommended-to-use-the-base-html-tag
[9]: https://github.com/yeoman/generator-angular/issues/433