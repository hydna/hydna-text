# Quick Start

The easiest way to get started is with a real working example. In this
introductionary guide we'll walking you through the creation of a simple chat
application in JavaScript.

Download the [project](https://github.com/hydna/hydna-chat) and use it as a
base.

**Note:** you'll need to access the html-document through a local web server
if you use the flash-fallback.

**Note:** this demo uses the open demo stream *demo.hydna.net/2222*. Register
your own domain if you do not want to receive other users' chat messages. You
can also use a random channel (1-4294967295) to reduce this risk.

## 1. Include the JavaScript library

First and foremost you need the client library. We recommend including the
file directly from our static file server, but you can of course download and
host your own local copy.

    :::html
    <script type="text/javascript" src="http://static.hydna.net/js/hydna.js">

## 2. Open a stream

Now open a stream in `rw` (read and write) mode. You should replace the
*domain* and *channel* (*demo.hydna.net* and *2222* respectively) used
below with your own.

    :::javascript
    var stream = new HydnaStream('demo.hydna.net/2222', 'rw');

## 3. Listen for messages

Add a callback that will be triggered as messages arrive on the stream. We're
using a simple JSON protocol to distringuish chat messages from join
announcements.

    :::javascript
    stream.onmessage = function(message) {
        var packet = JSON.parse(message);
        switch(packet.type) {
        case 'join':
            chat.infoMessage(packet.nick + ' has entered the chat!');
            break;
        case 'msg':
            chat.chatMessage(packet.nick, packet.message);
            break;
        }
        // scroll to bottom of chat. this could be disabled when the user
        // has manually scrolled.
        chat.attr('scrollTop', chat.attr('scrollHeight'));
    };

## 4. Send messages

Messages written to the stream will be received by all clients that are
listening for data on the same stream.

    :::javascript
    stream.send(JSON.stringify({
        nick: nick,
        type: 'msg',
        message: input.val()
    }));
