# websocket bench

Nodejs cli tool for benchmarking websocket servers. Currently supports:
* [Socket.IO](https://github.com/LearnBoost/socket.io)
* [Engine.IO](https://github.com/LearnBoost/engine.io)
* [Faye](https://github.com/faye/faye)
* [Primus](https://github.com/primus/primus)

[![Build Status](https://travis-ci.org/M6Web/websocket-bench.png?branch=master)](https://travis-ci.org/M6Web/websocket-bench)


## Installation

    npm install -g websocket-bench

## Usage

Tip: You may find it useful to increase the maximum number of open file descriptors on your system during testing:

`ulimit -n 60000`

Simple example (using Socket.IO by default):

`websocket-bench -a 2500 -c 200 http://localhost:3000`

Simple example (using Primus):

`websocket-bench -t primus ws://localhost:8080`

command help

    Usage: websocket-bench [options] <server>

    Options:

      -h, --help               Output usage information
      -V, --version            Output the version number
      -a, --amount <n>         Total number of persistent connection, Default to 100
      -c, --concurency <n>     Concurent connection per second, Default to 20
      -w, --worker <n>         Number of worker(s)
      -g, --generator <file>   Js file for generate message or special event
      -m, --message <n>        Number of message for a client. Default to 0
      -o, --output <output>    Output file
      -t, --type <type>        Type of websocket server to bench(socket.io, engine.io, faye, primus). Default to socket.io
      -p, --transport <type>   Type of transport to websocket(engine.io, websockets, browserchannel, sockjs, socket.io). Default to websockets (Just for Primus)
      -k, --keep-alive         Keep alive connection
      -v, --verbose            Verbose Logging


## Benchmark message

For benchmark message or more advanced connection you should provide your own `generator`

generator structure :

    (function() {

       /**
        * Before connection (optional, just for faye)
        * @param {client} client connection
        */
       exports.beforeConnect = function(client) {
         // Example:
         // client.setHeader('Authorization', 'OAuth abcd-1234');
         // client.disable('websocket');
       };

       /**
        * On client connection (required)
        * @param {client} client connection
        * @param {done} callback function(err) {}
        */
       exports.onConnect = function(client, done) {
         // Faye client
         // client.subscribe('/channel', function(message) { });

         // Socket.io client
         // client.emit('test', { hello: 'world' });

         // Primus client
         // client.write('Sailing the seas of cheese');

         done();
       };

       /**
        * Send a message (required)
        * @param {client} client connection
        * @param {done} callback function(err) {}
        */
       exports.sendMessage = function(client, done) {
         // Example:
         // client.emit('test', { hello: 'world' });
         // client.publish('/test', { hello: 'world' });
         done();
       };

    })();

## See also

French article about websocket-bench : [Benchmarking websockets avec Node.Js](http://tech.m6web.fr/benchmarking-websockets-avec-nodejs)
