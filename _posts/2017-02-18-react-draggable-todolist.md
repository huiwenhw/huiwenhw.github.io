---
layout: post
title: React Starter Kit
category: normal
---

I had to create a draggable & sortable todo list app, and I've tried React once using JSBin, so I figured it would be better to learn what's 'behind the scenes' so I know what's going on. Found this [tutorial online](http://andrewhfarmer.com/build-your-own-starter/#0-intro) & I'm gonna jot down notes from it! 

P.S. This post is not on the React framework, its on the configuration of tools before starting on React.

## NPM 

We add a package.json file to record our npm dependencies. Stuff that we add in the later steps are npm packages: Babel, Webpack, React etc. Adding a "node_modules" in .gitignore tells git to ignore the "node_modules" folder. It can be auto generated so we don't need it! Since its now an npm project, the "npm install" command can be used to install all dependencies. 

## Babel 

Babel transforms the ES6 + JSX JS that we'll be writing into ES5 JS that browsers can understand. We'll write in a special syntax called JSX, which lets you put HTML-like code inside Javascript.

## Webpack 

Webpack bundles all our JS together into a single file. This includes all the JS file that we write & the npm packages. Reason: A single .js file is easier to deploy & usually downloads faster than multiple small files. Webpack will work with Babel to convert our code from ES6/7 -> ES5 while we work. Webpack also minifies our .js file for production. 

babel-loader is a Webpack 'loader'. It supports running Babel from Webpack. 

## Express 

A React dev env needs some way to show your app in a browser => which means, we'll need a server. We'll set up Express as our server. 

Main file added here is server.js: It will run an Express server, which will run Webpack. Webpack will rerun whenever we change one of our JS files. 

server.js is server-side JS. We can run it with the node server.js command. We'll be adding a start script to run the server tooooo

Start by creating www/index.html as our first web page. Express is configured to serve all the files in the folder. 

## React

Finally, we add the React npm package! 
For websites on learning React, I think the official [doc](https://facebook.github.io/react/docs/hello-world.html) is worth a read!
