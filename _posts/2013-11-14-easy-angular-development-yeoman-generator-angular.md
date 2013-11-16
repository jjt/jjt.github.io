---
published: true
title: "Easy Angular development with Yeoman and generator-angular"
layout: post
---

One of the best things to happen in web development over the past few years has
been the variety of tools built on [Node.js][1001] and made available through
its package manager, [npm][1000]. Among the 75,000 or so modules, there are
three that I use almost every hour of every working day: [Grunt][0], a
versatile task runner; [Bower][1], a frontend asset package manager; and
[Yeoman][2], a generator framework that automates repetitive scaffolding and
dev/deploy tasks, using Grunt and Bower.

This article will focus on using Yeoman and [generator-angular][3] to set up a
great environment for developing Angular applications. With only a handful of terminal
commands, we'll have the following features: 

- Development server (connect) with livereload
- Unit and integration testing framework (karma)
- Choice between Javascript (default) and Coffeescript
- Project scaffolding (index.html, scripts/style/images directories, etc)
- Generators for routes, controllers, views, directives, etc.
- Sass/Compass and Coffeescript watch & compilation
- Production build task (uglify, css min, static versioning, concat, etc)

If Angular isn't your thing, there are also [dozens of other Yeoman
generators][5] that are worth checking out.

## Getting started

If you've never used Yeoman and/or it isn't installed, follow their
[instructions][4] and read up on what it does. If you don't have an `npm`
command, that likely means you don't have Node.js installed. I'd recommend
[nvm][1015] if you're on Linux/Mac, or the Windows installer from [the Node.js
homepage][1001].

Once we have Node.js and Yeoman, we'll install the generator and use it to scaffold our application by answering some questions about our project.
We're going to go with the defaults for this install, but on future projects feel free to ditch
Bootstrap. However, I'd recommend keeping the default Angular modules unless you
have a compelling reason to strip them out.

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

After answering these configuration questions, Yeoman will take a minute to 
scaffold our project files and pull down npm and Bower modules. Once that
finishes, we'll have our development environment all set
up.

Here are some relevant directories/files that Yeoman made for us:

    ▾ app/               # Main directory for application
      ▾ scripts/         # Angular js/coffee directory where most of our
        ▸ controllers/   #   yeoman-generated modules will go 
        app.js           # Main application js file
      ▸ styles/          # css/scss
      ▸ views/           # Angular views
        index.html       # Entry point to our single page app
    ▸ test/              # Tests (shocking, I know)
      bower.json         # Bower configuration file
      Gruntfile.js       # Grunt configuration file
      package.json       # Node/npm configuration file

## Grunt work

Assuming everything went smoothly, we should be able to start our server with Grunt.
Once you run `grunt server` bunch of tasks will complete and then Grunt will
run the "watch" task.

    $ grunt server
    # ... Grunt will start a bunch of tasks 
    Running "watch" task
    Waiting...

Somewhere before Grunt enters the "watch" task, it will open a browser tab pointed
to `http://127.0.0.1:9000` which should be the front page of our app: some
chipper dialogue with a big, green "Splendid!" button.

<p>
  <a class="imglink" href="/assets/posts/images/ng-generator-1.png">
    <img src="/assets/posts/images/ng-generator-1.png">
  </a>
</p>

At this point, we have our development server running and Grunt is monitoring
our project for changes. If we modify any project files (js, coffee, (s)css, html,
etc.) Grunt will
recompile as approprite and livereload will refresh the browser
automatically. If one of our tests change (or we touch the file),
Grunt will launch Karma to run it.

All of the features promised in the intro are now available to us and we can
focus on developing our app, which is a great place to be after issuing just 5-10
commands. Yeoman and Grunt work hard so we don't have to.

## Let it all Ang out

The focus of this post is the Yeoman generator so some Angular knowledge is
assumed for this section. As long as you're familiar with MVC frameworks you
should be able to follow along. If you're looking for more how-to-Angular, I
recommend the classic [Egghead.io videos][6].

When we ran `yo angular myApp`, we were using a synonym of `angular:app`, which
is one generator out of a dozen or so available to us. We can get the list of
available generators by typing `yo --help`, but here's a better reference:

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

We're going to use a few of them now to create a new `foo` route, with a
matching controller and view. Since we want all three to be named `foo`,
we can take a shortcut by just using the route generator, which will call the
controller and view generators in turn.

    $ yo angular:route foo
      invoke   angular:controller:/path/to/myApp/node_modules/generator-angular/route/index.js
      create     app/scripts/controllers/foo.js
      create     test/spec/controllers/foo.js
      invoke   angular:view:/path/to/myApp/node_modules/generator-angular/route/index.js
      create     app/views/foo.html

So what did this do for us?

- Created a `foo` controller with a matching test
- Created a `foo` view
- Added a `foo` route in the main `app.js` file, lacing up the controller and view
- Added a `<script>` tag to `index.html` to load the controller

If we were to go to `http://127.0.0.1:9000/#/foo` now, we would see the
stunningly beautiful sight of "This is the foo view" on a blank page. At this point
we would edit the files we just created to make our foo page into something.

The other angular:_______ generators are much the same in their functionality - they exist to
save time by handling all the monotonous scaffolding tasks.

## Smash the hash: Angular's html5mode and htmlPushState

You might have noticed that we used a hash in our url routing, when going to `/#/foo`.
This is the way
that Angular ships by default and it's completely fine, but for us pickier devs there's the [html5Mode][7]
setting which uses htmlPushState with a fallback shim for
unsupported browsers. Open up `app.js`, add the `$locationProvider` service to the
`config` function, and tell Angular to use html5Mode:

    .config(function ($routeProvider, $locationProvider) {
      $locationProvider.html5Mode(true);
      //...
    })

Now if we visit our app and click on a link to `/foo` for example,
it should change to the 'foo' view just as it did with the hash before.
Everything will work great when clicking around in our app, but once we start making
changes to our app, livereload will refresh the browser and it will 404. Huh?

That's happening because our connect server doesn't know how to serve anything
other than `/` and static files. So it sees `/foo` and looks for that file, which
doesn't exist. If you've
done any ops work using Apache or Nginx, (mod_)rewrite should spring to mind as the solution.
We'll be using the connect middleware `connect-modrewrite` for this job.

First we'll install it, saving it to our package.json:

    $ npm install --save-dev connect-modrewrite

And then we'll add the following snippet to our Gruntfile in the
`connect.options` object in the large `grunt.initConfig` function call, right
around line 60. This middleware rewrites any request that isn't for a valid static
file to `index.html`.

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

Alright, now we reload our browser at `http://127.0.0.1:9000/foo` and it loads
(hurray!), but our Angular app isn't initializing and the page is unstyled. We
can pop open our browser's network inspector and we'll see a bunch of 404s. 

This is because generator-angular assumes that we'll be using the hash mode and
generates relative paths like:

    <script src="scripts/app.js"></script>

So the browser is looking for `/foo/scripts/app.js`, which obviously doesn't exist.
I'm a fan of root-relative urls, so I go into `index.html` and turt every script
and link tag into root-relative ones:

    <script src="/scripts/app.js"></script>

Another option to the relative urls is to set the base url of the site by
putting `<base href="/">` in the head, but that's not without [its
problems][8]. There is [some discussion][9] on the generator-angular issue
tracker about adding html5mode support, or at least adding the option of a
prefix on the script/link tags (either just a slash for root-relative, or a full
absolute base url).

Note that enabling html5mode support also requires us to have a production
server that can do the rewriting. One of the advantages of Angular is that
we can ordinarily do a static deploy to S3 or another static server, reducing
devops work and server cost. So it's up to you to decide whether or not
html5mode is worth it.

## Fly my pretties!

Once we've developed our app and you want to deploy a release, we can do so
with the `grunt build` command. It will run a whole bunch of handy tasks, like
minify/uglify, jshint, ngmin, concatenation, etc. Once it's successfully done,
it will result in a production-ready build under the `dist` directory. We can
now push the contents of that directory to a static server and baby, we got a
stew goin!

I hope this article has shown how Yeoman and generator-angular can help you to
more quickly and efficiently develop Angular applications.


[1000]: https://npmjs.org/
[1001]: http://nodejs.org/
[1015]: https://github.com/creationix/nvm
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
