# Quick Start

In this example we'll be creating a simple chat. 

## Prerequisites

Before connecting to Hydna you should register your own *domain*. You can,
however, use our open demo-domain for the purpose of making yourself
acquainted with the concepts.

## 1. Include the JavaScript library

First and foremost you need the client library. We recommend including the
file directly from our static file server, but you can of course download and
host your own local copy.

    :::html
    <script type="text/javascript" src="http://static.hydna.net/js/hydna.js">

## 2. Open a stream

Now open a stream in `rw` (read and write) mode. This is done using your
*domain* and a *stream address*.

    :::javascript
    var stream = new HydnaStream('demo.hydna.net/1235', 'rw');

## 3. Listen for messages

Add a callback that will be called when data arrives on the stream.

    :::javascript
    stream.ondata(function(data) {
        var packet = JSON.parse(data);
        chat.append('<p><span>' + packet.from + '</span>' + packet.message + '</p>');
    });

## 4. Send messages

Messages written to the stream will be received by all clients that are
listening for data on the same stream.

    :::javascript
    var packet = JSON.stringify({
        from: username,
        message: message
    });
    stream.write(packet);

## What next?

Check out ...
