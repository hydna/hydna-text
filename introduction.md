# Introduction

**Hydna is a scalable, easy-to-use, hosted platform that enables developers to
send and receive messages in real-time. A large set of interfaces and client
libraries makes communication across platforms (browsers, operative systems)
and devices (computers, handhelds, smartphones) trivial.**

Real-time-enabling a site or application is the matter of including a client
library and writing a few lines of code; there's no need to write or install
server side software.

The basic concept is that clients open bi-directional
[streams](#section-streams) and exchange messages. A prolific example would be
a chat where clients would open a stream (`mychat.hydna.net` for example).
When a message is written to the stream, all clients that have opened the same
stream would receive it.

**Note**: while most all of the examples in the documentation section are
written in JavaScript, they should be transferable to other programming
languages.

Example of a JavaScript client using the [JavaScript Client
Library](http://www.hydna.com/documentation/implementations/hydna-js/):

    :::javascript
    var stream = new HydnaStream('demo.hydna.net', 'rw');
    stream.ondata('data', function(data) {
        // alert('data received on stream!');
    });
    stream.write('Hello, me!');

## Key Concepts

A few of the key concepts -- or components -- of Hydna are outlined below.
Developers should familiarize themselves with these concepts to featherbed
further investigations.

### Streams

Hydna is built around the concept of *streams*. A stream is a connection
identified by an *address* that can be used to send and receive messages (it
can also be used to send and receive signals, more about that below).

An important distinction to make is that a stream is not synonymous with a
connection; there can be hundreds of open streams over the same connection.

When you open a stream you need supply a few parameters: a *domain*, an
optional *channel* and the mode in which you wish to open the stream.

Streams are typically recognized as a *URI* following the format
`<domain>[:port][/channel]`.

You encountered the *URI* `demo.hydna.net/1234` in the example above. 

* A *domain* is an identifier for an isolated "zone" in Hydna. It's also,
  incidentally, a fully qualified domain name to which a connection will be
  made.
* A *channel* is identified number in the range 1-4294967295 that is used to
  uniquely identify a stream (much like ipv4 addresses). The channel ids are
  unique per *domain*.

Streams can be opened in three major *modes* depending on what connecting
clients plan -- or are allowed to -- do over them:

- `read` mode implies the ability to read messages sent over the stream.
- `write` mode implies the ability to write messages to the stream.
- `emit` mode implies the ability to emit signals over the stream.

**Note**: generally only the first letter of a mode is used when it is
referenced, not much unlike filesystem permissions: `r` denotes `read`, `w`
denotes `write` and `e` denotes `emit`.

Modes can also be combined; a stream can be opened in `rw` or `rwe` mode for
example.

**Note**: Open streams will always receive signals, regardless of mode.

### Signals

Signals are much like regular messages, but are sent to the *behavior
instance* instead of being routed to other clients listening for data on a
stream. This is useful when you need access to preserved state or server-side
logic.

### Behaviors

Behaviors are one of the pillarstones of Hydna and can be used to set up rules
for streams and how Hydna should behave when streams are opened or when
signals are dispatched. While the most typical use-cases can be run without
involving behaviors, they can be used to unlock great control and in many
cases replace the need for hosting server logic.

Behaviors are typically used to implement authentication, logging and more.

### Architecture

*Transport proxies* are responsible for receiving, routing and transporting
messages.

*Behavior instances* are allocated per *domain* and are responsible for
keeping states, closing streams, denying or authorizing specific uses of
streams, dealing with signals and more.

### Transports

A transport is a means of transfering data over Internet. There are a few
different transports clients can use to exchange messages over Hydna; each
with their individual traits and foibles:

- *Hermes Binary Protocol* (fast, not supported in browsers without using
  Flash)
- *WebSockets* (fast, not supported by all browsers and devices)
- *HTTP Comet* (slightly slower but compatible with almost anything)
- *HTTP REST* (limited to pushing, but extremly easy to implement without a
  client library)

Which transport layer is used will be transparent to most developers (if you
use a client library, the choice of transport has already been made), but it's
an important architectural feature that developers should be aware of.

The characteristics of the environment determines which transport is most
suitable (WebSockets are great and a Flash-fallback can be used, but neither
works in popular Apple devices. Comet works with almost any HTTP client that
supports JavaScript, but is not as efficient as using Web Sockets) and it's up
to the client library developer to choose the right transport. In some cases
multiple transports might be needed -- the JavaScript implementation, for
instance, automatically detects if it should use native Web Sockets, Flash or
HTTP Comet.

