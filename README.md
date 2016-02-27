# From Ruby to Node.js: A Brief Introduction

## What is Node.js?

So for those of you that **actually** care about the history of things, you
should probably read the [node.js wiki](https://en.wikipedia.org/wiki/Node.js) to get a proper in depth discussion on it. For everyone else, Node was originally written in 2009 by Ryan Dahl because he figured there had to be a better way to see the progress of a file upload without having to query the web server. As with most programmers, he decided to come up with a solution. In this case, his solution ended up being what we are learning about today!

The only other notable dates to speak of are the introduction of [npm](https://www.npmjs.com/) in 2011, which is the package manager for the node community and a brief *fork* of node.js in 2014 called io.js that wanted an open governance alternative to node (since at the time node was governed by Joyent) along with keeping up to date with the [Google V8](https://developers.google.com/v8/) JavaScript engine. Luckily, everybody shook hands and reformed under the Node.js Foundation.

### That's cool. I Still don't know what Node.js is!

Alright, now it is time for the question that stumps many of folk. What exactly is Node.js? Node is **NOT** a programming language. Node is **NOT** a framework. It is a cross-platform runtime environment. Since that probably sounds a little confusing right about now, let's break Node apart into the basic components that make it up. That is JavaScript, V8 and the event loop. First up is JavaScript.

### JavaScript, yeah, that thing.

JavaScript is a programming language. It has curly braces and semi-colons. Also, it has slowly become one of the most used languages in the world due to it being the language of the web. We write our node applications using the javascript language, but node itself is not completely written in javascript. The great thing about javascript is that it is really good for event driven programming. Programming for the web deals with tons of events, so why not use javascript as the language for implementing an event-driven server side implementation? The only problem is that historically javascript had only been written for the web. So, how was that rectified?

### V8! Good for muscle cars, good for javascript!

Browsers are applications. They need to interpret that javascript one way or another. How do they do it? Using javascript engines. Who happens to make a pretty decent javascript engine? Google! So node uses that engine to compile your javascript code down to native machine code. So that's two-thirds of the equation. Last, but certainly not least is the event-loop and event driving programming.

### Waiter! Can we get more drinks?

Let's start with a little story example. Imagine we have two waiters named Ruby and Node. Ruby goes to a table of ten and takes the order of one and only one person from that table. It then goes and drops the order ticket off at the kitchen. There's only one problem. Ruby likes to talk it up with the chef while he cooks, so ruby waits, and waits, and waits until the food is ready, then returns the plate to the customer. Rinse and repeat for each customer.

Node, on the other hand is a fantastic waiter. Node takes the order of one customer, goes to the kitchen to drop off the ticket, then heads right back to the table to take the next customers order. Node knows that it is going to take time to cook the food so it can get other things done in the interim. When one of the orders is done, the kitchen rings the bell to let Node know that it can come back and bring the order back to the customer.

What was just described was the difference between a blocking-synchronous model (Ruby) & a non-blocking asynchronous model (Node). How does Node do this? Using an event loop that is written in C++. This can get very low-level real quick so here is the basic gist. The event loop keeps track of any currently executing I/O requests. When one of the I/O requests are finished, the event loop fires off an event that triggers node to react to that event with some code commonly called a callback. We'll get back to callbacks later.

## Let's (kind of) Get Started

So the first thing that you need is, wait for it... NODE! So let's go ahead and install it. There are two ways we could do this:

* Go straight to the [node.js](https://nodejs.org/en/) website and download the latest version from the home page.
* For those familiar with using a version manager like rvm, there is a version manager for node called [nvm](https://github.com/creationix/nvm). We'll quickly go through installing nvm

### Installing nvm

It is pretty simple to set up, all we need to do is run this curl command on the command line:

```bash
  $ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash
```

Once that is finish installing, you may need to refresh your terminal session:

```bash
  $ source ~/.bash_profile
  or
  $ source ~/.zshrc
```

To view all the available version of node you use the command:

```bash
  $ nvm ls-remote
```

For our purposes, let's go ahead and install the latest version of node. If we pass nvm the keyword node, it automatically downloads the latest version:

```bash
  $ nvm install node
```

There are of course many other commands that we can use, but for our purposes we are good to go!

### The node command

Just how ruby has ```irb``` (or ```pry```) node gives use the ```node``` command that gives us a REPL (Read Eval Print Loop) for messing around and breaking shit.

```bash
  $ node
```

That's cool and all, but not being able to save anything is mad wack. Let's create our first node script! Oh yes, you know what's coming.

### Hello Turing?

In the directory of your choice go ahead and ```touch``` the file ```hello_turing.js``` (or don't. I really don't care).

Inside ```hello_turing.js``` insert this code:

```js
  console.log('Hello Turing!');
```

To run the script, run this command:

```bash
  $ node hello_turing
```

Marvel at your beautiful creation! My work here is done! Err... wait. You want more? Fine. You asked for it!

## Even more Hello Turing!

That was fun! (not really). Let's crank it up a notch and read in a file and print its contents to the terminal.

### Starting a new node project

When you installed node it came with another awesome program called [npm](https://www.npmjs.com/). Npm is what manages all your dependencies. For rubyist, this is equivalent to the ```gem``` command. Besides for being able to install modules, it also gives us a useful command for setting up a node project. Go ahead and type:

```bash
  $ npm init
```

This command will walk you through creating a ```package.json``` file. The ```package.json``` file will hold info about your application like the version, git repository and author. Along with this, the ```package.json``` also keeps track of any dependencies that your application will need.

If you open up the file in your text editor, you may or may not be familiar with this odd looking format. This is JSON (JavaScript Object Notation). All I will say about this is get used to seeing it, because it is literally used everywhere!

### Requiring modules and weird, awesome ES2015 (ES6)!

Node is very lightweight out of the box. It doesn't include access to the file system straight away, so we need to require it within our file. Go ahead a delete everything in the ```hello_turing.js``` file and write this code:

```js
  const fs = require('fs');
```

Let's break this down. JavaScript is constantly being updated with new features and syntax updates. One of the newest editions was ```const``` and ```let```. Before, there was just the lonely ```var```. Now, if we know that a variable won't be reassigned we would use ```const``` instead. Const is just shorthand for constant. ```let``` signals that the variable may be reassigned. ```var``` has become the weakest of the three and varies in usage. There is no real standard around the usage of the three, but the majority of applications that I have seen using ES2015 tend to lean towards using ```const``` and ```let```. Requiring modules in Node is as straight forward as it looks. The only time it gets a little tricky is when you create your own modules. Let's say we created a module in our project called ```file_loader.js```, we would require that file in our project like so:

```js
  const fileLoader = require('./file_loader');
```

Now that we have access to our file system, let's write some code to read a file and print out the results to the console:

```js
  fs.readFile('hello.txt', 'utf8', (err, data) => {
  if (err) { throw err; }
  console.log(data);
});
```

There is some odd looking things here, let's breakdown the more glaring things.

```utf8```: If you do not pass in an encoding for the file that is being read, then node will pass in a buffer object. What the hell is a buffer object you say? Buffer objects are designed to handle raw binary data. Node at one point did return just straight up strings, but that proved to be very slow when it came to dealing with binary data such as TCP streams and reading and writing to the filesystem. So, by default node now return a buffer object.

```js
  (err, data) => {}
```

This is the new syntax to create an anonymous function in JS. You may be familiar with the older syntax:

```js
  function(err, data) {}
```

Just some new syntatic sugar. What is more interesting is why there is a function there? This is a callback function. Node being an event-driven language, it responds to events in our program. When an event fires off, it executes the chunk of code that is within the callback function. It is convention for callbacks in node to send the error message as the first parameter and the data as the second parameter. The event in this case would be when the file is finished being read. The code itself is pretty straightforward after that, so this is what your file should look like at the moment:

```js
  const fs = require('fs');

  fs.readFile('hello.txt', 'utf8', (err, data) => {
    if (err) { throw err; }
    console.log(data);
  });
```

Go ahead and create a ```hello.txt``` file with the contents of 'Hello Turing!' and run ```$ node hello_turing``` and you'll see the output in the terminal.

### Quiz time!

Let's add this line of code right after our file reading code:

```js
  console.log("Hold up, wait a minute?");
```

What ```console.log();``` gets run first? Why? If your answer is because node is
asynchronous then you would be totally correct. This is a perfect example of node's
event loop in action. Though we can't perceive the time it takes to read a file in
this small application, behind the scenes there is an ever so slight wait for node
to get back this file. So while node waits, it will go on to do other things. In
this case it sees the other ```console.log()``` and runs that while it is waiting on
file to be read.

This has both advantages and disadvantages. One advantage is that if you are
creating an application that has to deal with a large amount of incoming I/O, nodes
non-blocking pattern makes it perfect for the job. The one drawback is that coding
in a asynchronous matter can get a little messy. Sometimes you end up with nested callback
hell or sometimes your code doesn't run the way you expect it to run. There are
patterns to help you wrap your head around programming in an asynchronous way
(Promises, PubSub, etc..) but be aware that there is that initial learning
curve to be aware of.

Tired of the command line? Let's get some text up on the web!

## Even more Hello Turing! Now with Interwebs!

So hopefully at this point you are starting to get a little more comfortable with
node. So I'll take off the training wheels and ramp up things just a bit. If you
are following along, go ahead and create a new directory called ```simple_server```  
initialize a new node project with the default values. Then ```touch``` a file named
```server.js``` and open it up in your text editor.

For this exercise we are going to be using the framework [express](http://expressjs.com/)
which is a minimalist web framework. Think of it as Sinatra for node. In order to
get express, we want to ```npm install``` it. The difference now is that express is
an external module that is going to be a dependency of our app. To save a dependency, we
run the ```npm install``` command with the ```--save``` flag like so:

```js
  $ npm install express --save
```

Once that has finish downloading you will have noticed two changes to your project.

1. If you were to take a look at your ```package.json``` file, you will notice there
is now an added section called "dependencies". Similar to your Gemfile in a ruby
project, you house all the dependencies that are needed in here.

2. There is also a ```node_modules``` folder that has been added. This is where
all the files are housed for our dependencies.

I'm going to go ahead and give you all the code straight away and let's just openly
talk about what is going on (because this README is getting to damn long!)

```js
  'use strict'

  const express = require('express');
  const app = express();

  let port = 3000;

  app.get('/', (req, res) => {
    res.send('Hello Turing!');
  });

  app.listen(port, () => {
    console.log('Example app listening on port 3000!');
  });
```

## Simple Slack: Real-Time Web For Beginners
* Not sure if I'll get to this

## Other Cool Node Links
* [Node.js](https://nodejs.org/en/)
* [NPM](https://www.npmjs.com/)
* [Node School](http://nodeschool.io/)
* [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)
