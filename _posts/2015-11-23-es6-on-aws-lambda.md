---
layout: post
title:  "ES6 on AWS Lambda"
date:   2015-11-23 10:00:00
categories: es6 aws lambda nodejs
---

[AWS Lambda](https://aws.amazon.com/lambda/) is a very cool and simple service
able to run "cloud functions" very fast and cost-effectively. It is particularly
interesting if you want to process events that spawn into your AWS
infrastructure.

Originally, Lambda was based on Node.js. Now lambda covers more languages with
Python 2.7 and Java 8. Unfortunately, Lambda doesn't seem to have a really
up-to-date Node.js engine preventing us from using
[Node's ES6 capabilities](https://nodejs.org/en/docs/es6/).

With [Babel](https://babeljs.io) and [Webpack](https://webpack.github.io), we'll
be able to have all of the ES6 features running on Lambda.

**TL;DR;** Check out the
[example repository](https://github.com/rricard/lambda-es6-example) to run
lambdas with ES6.

## Setup webpack & babel

### Initialize and dependencies

First thing we need to do is to
[setup a basic npm package.json](https://github.com/rricard/lambda-es6-example/commit/1f61676b8972a28593a1f3d8113e0792472eac90).
It's as simple as running `npm init`.

Then, we'll need to
[import webpack, babel and its presets and plugins](https://github.com/rricard/lambda-es6-example/commit/a263795009234b0f7fecdc92220f851d509559eb).

```
npm i --save-dev webpack babel-core babel-loader json-loader \
  babel-preset-es2015 babel-plugin-transform-flow-strip-types \
  babel-plugin-syntax-flow
```

Note that in this example I import the webpack's `json-loader` to allow requires
on JSON files. I also import the Babel [flowtype](http://flowtype.org) plugins
to allow the use of flow type-checking annotations.

### Configure webpack

Configuring webpack is trickiest part of this tutorial but once explained,
there's no magic needed, don't worry!

Let's start with a
[basic `webpack.config.js`](https://github.com/rricard/lambda-es6-example/commit/f7787b3c24fc332f35d7117489a9eaefe4ea1afc):

```js
// webpack.config.js

var path = require("path");

module.exports = {

};
```

#### Module Loaders

I usually start my webpack config with the
[module loaders](https://github.com/rricard/lambda-es6-example/commit/bccdaf2586350604d27c4fca9ae8a8ffe9375822):

```js
module: {
  loaders: [
    {
      test: /\.js$/,
      exclude: /node_modules/,
      loader: 'babel',
      query: {
        presets: ['es2015'],
        plugins: ['syntax-flow', 'transform-flow-strip-types']
      }
    },
    {
      test: /\.json$/,
      loader: 'json'
    }
  ]
}
```

Ok, if you don't know webpack, nothing really complicated here: each js file
imported will be transformed through babel with the es2015 preset while
stripping the flow annotations. The json files will be imported like Node
would do it.

#### Output

Then, we can define
[how and where we want webpack to output my files](https://github.com/rricard/lambda-es6-example/commit/cf6e5dce81c34630d67b333262ef6ac4c83f040e):

```js
// webpack.config.js

var path = require("path");

module.exports = {
  output: {
    path: path.join(__dirname, "dist"),
    library: "[name]",
    libraryTarget: "commonjs2",
    filename: "[name].js"
  },
  target: "node",
  module: { loaders: [ /* ... */ ] }
};
```

Here are some changes between the usual output you configure when you do a web
project and what we intend to do here:

- We'll have multiple output files (one by lambda), so we need to use the
  `[name]` placeholder in our config.
- We'll need to expose those files as correct node modules, that's why `library`
  and `libraryTarget` are essential.
- We also need webpack to ignore native node deps such as `fs` or `http`, that's
  why we use the node `target`.

Don't forget to add the `dist/` directory in your `.gitignore`!

#### Entry points

Since we'll have an undefined number of lambdas, we want to list a directory
to
[use all of the files in this dir as entry points](https://github.com/rricard/lambda-es6-example/commit/8ee9824857f734d77a5cfa95b3fea6d1947a7e00).

To that end, we need to read the directory synchronously and transform the
result to something that looks like this:
`{lambda1: "./lambdas/lambda1.js", lambda2: "./lambdas/lambda2.js"}`.

```js
// webpack.config.js

var path = require("path");
var fs = require("fs");

module.exports = {
  entry: entry: fs.readdirSync(path.join(__dirname, "./lambdas"))
         .filter(filename => /\.js$/.test(filename))
         .map(filename => {
            var entry = {};
            entry[filename.replace(".js", "")] = path.join(
             __dirname,
             "./lambdas/",
             filename
           );
           return entry;
         })
         .reduce((finalObject, entry) => Object.assign(finalObject, entry), {}),
  output: { /* ... */ },
  target: "node",
  module: { loaders: [ /* ... */ ] }
};
```

Note that the precedent snippet uses arrow functions & `Object.assign()` so
you'll need to use a fairly recent node version to develop locally.

#### Try the compilation

Now, we can test a simple lambda function locally to
[test if webpack does its job correctly](https://github.com/rricard/lambda-es6-example/commit/cfb1b6d3bad6e70c99eea1903e6a0163e27fc0db).

You can now try the compilation with a simple lambda: `hello.js`

```js
// lambdas/hello.js

/* @flow */

type HelloOptions = {
  name: string
};

export function hello(options: HelloOptions, context: any): void {
  context.succeed(`Hello ${options.name || "world"}!`);
}
```

As you can see, this file uses ES6 & flow heavily.

We'll now be able to start a small script to ensure if we can execute our
lambda: `try-hello.js`

```js
// bin/try-hello.js

var helloModule = require("../dist/hello.js");

var fakeLambdaContext = {
  succeed: function succeed(results) {
    console.log(results);
    process.exit(0);
  }
};

helloModule.hello({name: "bob"}, fakeLambdaContext);
```

You can now run webpack and try to run the script:

```
./node_modules/.bin/webpack
node ./bin/try-hello.js
```
