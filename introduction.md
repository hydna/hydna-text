# Introduction

**Hydna is a scalable, easy-to-use, hosted platform that enables developers to
send and receive messages in real-time. A large set of interfaces and client
libraries makes communication across platforms (browsers, operative systems)
and devices (computers, handhelds, smartphones) trivial.**

Real-time-enabling your site or application is the matter of including a
client library and writing a few lines of code; we take care of everything
related to servers, scaling and maintenance.

*Note*: while most all of the examples in the documentation section are
written in JavaScript, they are transferable to other programming languages.

Example of a bi-directional JavaScript client:

    :::javascript
    var con = hydna.open('<customer>.hydna.net/1234', 'rw');
    con.addListener('data', function(data) {
        // alert('data received on stream address 1234!');
    });
    con.send('data!');

## Key Concepts

These are a few of the concepts -- or components -- of Hydna a developer
should be familiar with.

### Streams

A stream is a connection to Hydna that can be used to send and receive
messages (it can also be used to send and receive signals, more about that
below).

Streams are identified by a URI (hostname + stream address). Can be opened in
R, W or E mode.

### Messages

### Behaviors

Behaviors are one of the pillarstones of Hydna. 

Behaviors are powerful. No need to write a server. Can be used for logging,
authentication, backend plugins etc.

### Transports

A transport is a means of communicating over Hydna. There are a few different
transports clients can use to exchange messages over Hydna; each with
slightly different traits:

- Hermes Binary Protocol (fast, not supported in browsers without using Flash)
- Web Sockets (fast, not supported by all browsers and devices)
- HTTP Longpolling (slightly slower but compatible with almost anything)
- HTTP REST (limited to pushing, but extremly easy to implement without a
  client library)

The transport layer should be mostly transparent to "regular" developers (if
you use a client library, the choice of transport has already been made), but
it's an important feature of Hydna that you should be aware of.

Which transport to use depends on the nature of the application (WebSockets
are great and can use a Flash-fallback, but neither works in popular Apple
devices. Longpolling works with almost any HTTP agent, but is not as efficient
as using Web Sockets) and it's up to the client library developer to choose
the right transport. In some cases multiple transports might be needed -- the
JavaScript implementation, for instance, automatically detects if it should
use WebSockets, Flash or HTTP Longpolling.
