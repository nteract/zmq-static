## Deprecated: All features are now available in [`zmq-prebuilt`](https://github.com/nteract/zmq-prebuilt)!

[![Greenkeeper badge](https://badges.greenkeeper.io/nteract/zmq-static.svg)](https://greenkeeper.io/)

[![Build Status](https://travis-ci.org/nteract/zmq-static.svg?branch=master)](https://travis-ci.org/nteract/zmq-static)
[![Build status](https://ci.appveyor.com/api/projects/status/mos911y2mq3a7ag5?svg=true)](https://ci.appveyor.com/project/nteract/zmq-static)

**zmq-static**: Your statically linked [ØMQ](http://www.zeromq.org/)
bindings for [Node.js](https://nodejs.org/en/).

ØMQ provides handy functionality when working with sockets.

**zmq-static** will bundle ØMQ, allowing you to package ØMQ in your Node.js
or Electron aplication.


**zmq-static** simplifies creating communications for a Node.js
application by providing well-tested, ready to use ØMQ bindings.
zmq-static supports all major operating systems, including:

* OS X/Darwin
* Linux
* Windows


## Usage

Replace `require(zmq)` in your code base with `zmq-static`. That's it.

## Installation

To set up `zmq-static`, fork this repository and
clone your fork to your system. Be sure you have `git-lfs` and `python2` installed.


**Prerequisites for Linux and OS X**

If you are running on Linux or OS X, you will need to have `automake`,
`autoconf`, `wget` and `libtool` installed. For Linux, use your distribution's
package manager to install. On OS X, these can be installed using
[Homebrew](http://brew.sh) and using the Homebrew command `brew install`
command. For example, install `wget` with `brew install wget`.

**Prerequisites for Windows**

On Windows you'll need a C++ compiler, use on of the options provided
[here](https://github.com/Microsoft/nodejs-guidelines/blob/master/windows-environment.md#prerequisites).

Install `zmq-static` with the following:

```bash
npm install
```

## Testing

Run the test suite using:

```bash
npm test
```

## Running an example application

Several example applications are found in the `examples` directory. Use
`node` to run an example. To run the 'subber' application, enter the
following:

```bash
node examples/subber.js
```


## Examples using zmq-static

### Push/Pull

This example demonstrates how a producer pushes information onto a
socket and how a worker pulls information from the socket.

**producer.js**

```js
// producer.js
var zmq = require('zmq-static')
  , sock = zmq.socket('push');

sock.bindSync('tcp://127.0.0.1:3000');
console.log('Producer bound to port 3000');

setInterval(function(){
  console.log('sending work');
  sock.send('some work');
}, 500);
```

**worker.js**

```js
// worker.js
var zmq = require('zmq-static')
  , sock = zmq.socket('pull');

sock.connect('tcp://127.0.0.1:3000');
console.log('Worker connected to port 3000');

sock.on('message', function(msg){
  console.log('work: %s', msg.toString());
});
```

### Pub/Sub

This example demonstrates using `zmq-static` in a classic Pub/Sub,
Publisher/Subscriber, application.

**Publisher: pubber.js**

```js
// pubber.js
var zmq = require('zmq-static')
  , sock = zmq.socket('pub');

sock.bindSync('tcp://127.0.0.1:3000');
console.log('Publisher bound to port 3000');

setInterval(function(){
  console.log('sending a multipart message envelope');
  sock.send(['kitty cats', 'meow!']);
}, 500);
```

**Subscriber: subber.js**

```js
// subber.js
var zmq = require('zmq-static')
  , sock = zmq.socket('sub');

sock.connect('tcp://127.0.0.1:3000');
sock.subscribe('kitty cats');
console.log('Subscriber connected to port 3000');

sock.on('message', function(topic, message) {
  console.log('received a message related to:', topic, 'containing message:', message);
});
```

## Learn more about nteract

- Visit our website http://nteract.io/.
- See our organization on GitHub https://github.com/nteract
- Join us on [Slack](http://slack.nteract.in/) if you need help or have
  questions. If you have trouble creating an account, either
  email rgbkrk@gmail.com or post an issue on GitHub.

<img src="https://cloud.githubusercontent.com/assets/836375/15271096/98e4c102-19fe-11e6-999a-a74ffe6e2000.gif" alt="nteract animated logo" height="80px" />
