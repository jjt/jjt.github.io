---
published: false
title: "Angular html5Mode using Yeoman & generator-angular"
layout: post
---

## Smash the hash and use htmlPushState instead

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
(hurray!), but our Angular app isn't initializing and the page is unstyled. If we
pop open our browser's network inspector we'll see a bunch of 404s. This is
because generator-angular assumes that we'll be using the hash mode and
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
