---
published: false
title: "An easy start to modular Angular apps, with Yeoman and generator-angular"
layout: post
---

One of the best things to happen in web development over the past few years has been the variety of tools built on [Node.js][1001] and made available through its package manager, [npm][1000]. Three of the most useful command line tools are [Grunt][0], a versatile task runner; [Bower][1], a frontend asset package manager; and [Yeoman][2], a generator framework that automates repetitive scaffolding tasks. If you haven't used Yeoman before, follow [their instructions][4] to get up and running. If Angular isn't your thing, they also have [dozens of other generators][5] that are worth checking out.

This article will focus on using the [Angular generator][3] to get a full frontend application development environment going with the following features:

- Development server (connect) with livereload
- Unit and integration testing framework
- Choice between Javascript (default) and Coffeescript
- 
- Modern index.html with Bootstrap
- Sass/Compass, and Coffeescript watch & compilation
- Production build (uglify, css min, static versioning, concat files, etc)

## Installing generator-angular and instantiating ourApp



```shell
$ mkdir ourApp && cd ourApp
$ npm install -g generator-angular
$ npm install generator-angular
$ yo angular ourApp (--coffee)
```

At this point, you should 

[1000]: https://npmjs.org/
[1001]: http://nodejs.org/
[0]: http://gruntjs.com/
[1]: http://bower.io/
[2]: http://yeoman.io/
[3]: https://github.com/yeoman/generator-angular
[4]: http://yeoman.io/gettingstarted.html
[5]: http://yeoman.io/community-generators.html

