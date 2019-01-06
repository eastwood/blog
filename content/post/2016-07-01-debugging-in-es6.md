---
title: Debugging in ES6
date: 2016-07-01
tags: ["javascript", "node", "debug"]
type: "post"
---

In the advent of ES6 and transpilers, writing clean and concise javascript has never been so easy and kind on the soul. However, using a compiler presents some challenges when it comes to debugging, namely in relation to mapping the source code to the compiled output. Fortunately, this isn't a new problem and has been solved for plenty of other compilers. In this post, we'll focus on javascript's most popular compiler, [babel](http://babel.org) and the tools to use to enable this.

### Why use babel?
Node 6 is pretty [close](http://node.green/) to implementing all of ES6 proposed features, but in the current state of things we are missing one important feature; *modules*. If you'd prefer to use CommonJS modules instead of ES6 modules, then you will probably get away with just using native javascript and using `node debug main.js`. However, if you'd prefer to use babel and take advantage of es6 features, then read on.

### Setting up babel
Installing babel is dead simple, let's get started with an example project:

```json
# package.json
{
  "name": "es6-debug-sample",
  "version": "1.0.0",
  "description": "",
  "main": "src/index.js",
  "scripts": {
    "build": "babel --presets es2015 src -d dist"
  },
  "devDependencies": {
    "babel-cli": "^6.9.0",
    "babel-preset-es2015": "^6.5.0"
  }
}
```

Next, we're going to create basic file in `src/index.js`:

```javascript
import foo from './foo'

foo('hello world');

```

and `src/foo.js`

```javascript
export default function(str) {
  debugger; // this is going to breakpoint our debugger
  console.log(str);
}

```

Now let's run `npm run build`, we should see a new `dist` folder with the compiled output. For example `dist/index.js`

```javascript
// ES5 compiled output

'use strict';

var _foo = require('./foo');

var _foo2 = _interopRequireDefault(_foo);

function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }

(0, _foo2.default)('hello world');

```

Obviously, debugging this compiled output isn't the most readable. Let's see an example; if you run `node debug dist/index.js` you should get a debugger session like this:

```
< Debugger listening on port 5858
connecting to 127.0.0.1:5858 ... ok
break in dist/index.js:1
> 1 'use strict';
  2
  3 var _foo = require('./foo');
debug> cont
break in dist/foo.js:8
  6
  7 exports.default = function (str) {
> 8   debugger;
  9   console.log(str);
 10 };
debug> next
break in dist/foo.js:9
  7 exports.default = function (str) {
  8   debugger;
> 9   console.log(str);
 10 };
 11 });
debug> next
< hello world
break in dist/foo.js:10
  8   debugger;
  9   console.log(str);
>10 };
 11 });

```

This isn't ideal and though semantically reflects our source code, we can do better!

### Using node inspector
[Node Inspector](https://github.com/node-inspector/node-inspector) is a debugger interface for Node.js applications that basically uses Chrome dev tools to support the source mapping from `src` files to `dist` files. Node inspector does this by using source map files. First things first, install `node-inspector`:

```
$ npm install -g node-inspector
```

Next, we need to tell our compiler to output debugging information, in this case in the form of source maps. 

```
$ npm run build -- --source-maps
```

Now that we have the source maps compiled, we need to fire up node inspector to begin debugging the source files:

```
$ node-debug dist/index.js
```

Your web browser should automatically fire up (make sure you have chrome installed) and begin stepping through the code. The debugger will automatically stop at the `debugger` statement, but you can also set breakpoints in the dev tools as well.


<center><img width="600" height="400" layout="responsive" src="/images/es6-debug.png"></img></center>

### Summary
That's it! Now you can write your code in es6 for both the server and browser and debug it with chrome dev tools. 
Enjoy!
