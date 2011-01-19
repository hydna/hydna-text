# Quick Start

In this example we'll be creating a simple chat. 

## Include the JavaScript library

    :::html
    <script src="http://static.hydna.net/js/hydna.js">

## Connect to Hydna

    :::javascript
    var stream = new HydnaStream('http://demo.hydna.net/1235', 'rw');

## Listen for messages

    :::javascript
    stream.ondata(function(data) {
        var packet = JSON.parse(data);
        chat.append('<p><span>' + packet.from + '</span>' + packet.message + '</p>');
    });

## Send messages

    :::javascript
    var packet = JSON.stringify({
        from: username,
        message: message
    });
    stream.write(packet);
