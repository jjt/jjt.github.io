---
published: false
title: "Easy Angular development, with Yeoman and generator-angular"
layout: post
---

One of the best things to happen in web development over the past few years has been the variety of tools built on [Node.js][1001] and made available through its package manager, [npm][1000]. Three of my most-used command line tools are [Grunt][0], a versatile task runner; [Bower][1], a frontend asset package manager; and [Yeoman][2], a generator framework that automates repetitive scaffolding tasks.

This article will focus on using the [Angular generator][3] to get a full frontend application development environment going with the following features:

- Gruntfile and Bower
- Development server (connect) with livereload
- Unit and integration testing framework
- Choice between Javascript (default) and Coffeescript
- Project scaffolding (
- Sass/Compass, and Coffeescript watch & compilation
- Production build (uglify, css min, static versioning, concat files, etc)

## Getting started

If you haven't used Yeoman before, follow [their instructions][4] to get up and running. If Angular isn't your thing, they also have [dozens of other generators][5] that are worth checking out.

We're going to install the generator and answer some questions about our project. I'm going to go with the defaults for this install. Feel free to ditch Bootstrap, but I'd recommend keeping the modules unless you're quite familiar with how Angular works.

```shell
$ mkdir ourApp && cd ourApp
$ npm install -g generator-angular
$ npm install generator-angular
$ yo angular ourApp (--coffee)

[?] Would you like to include Twitter Bootstrap? (Y/n)
[?] Would you like to use the SCSS version of Twitter Bootstrap with the Compass CSS Authoring Framework? (Y/n)
[?] Which modules would you like to include? (Press <space> to select)
❯⬢ angular-resource.js
 ⬢ angular-cookies.js
 ⬢ angular-sanitize.js
 ⬢ angular-route.js
```
You'll now see approximately 20 metric assloads of output to the screen while Yeoman scaffolds out our project and pulls down npm and Bower modules. Assuming everything went OK, we should be able to start our server with Grunt:

    $ grunt server

This should open up a browser window with ourApp, which will look something like this:




```
▾ app/
  ▸ bower_components/
  ▾ scripts/
    ▾ controllers/
        main.js
      app.js
  ▾ styles/
      main.scss
  ▾ views/
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

