# Introduction

**Hydna is a scalable, easy-to-use, hosted platform that enables developers to
send and receive messages in real-time. A large set of interfaces and client
libraries makes communication across platforms (browsers, operative systems)
and devices (computers, handhelds, smartphones) trivial.**

Real-time-enabling your site or application is the matter of including a
client library and writing a few lines of code; we take care of everything
related to servers, scaling and maintenance.

**Note**: while most all of the examples in the documentation section are
written in JavaScript, they should be transferable to other programming
languages.

Example of a bi-directional JavaScript client:

    :::javascript
    var stream = new HydnaStream('demo.hydna.net/1234', 'rw');
    stream.ondata('data', function(data) {
        // alert('data received on stream address 1234!');
    });
    stream.write('Hello, me!');

## Key Concepts

These are a few of the concepts -- or components -- of Hydna a developer
should be familiar with.

### Streams

Hydna is built around the concept of *streams*. A stream is a connection
identified by an *address* that can be used to send and receive messages (it
can also be used to send and receive signals, more about that below).

An important distinction to make is that a stream is not synonymous with a
connection; there can be hundreds of open streams over the same connection.

Streams are typically recognized as a *URI* following the format:
`<host>[:port][/stream address][?token]`.

You encountered the *URI* `demo.hydna.net/1234` in the example above. 

- `<host>` is the host you connect to. You're supplied with one when you
  sign up for an account.
- `[:port]` is the port on which connections are established (default: 80, you
  should typically never need to specify a different port).
- `[/stream address]` address of the *stream* that is being opened. Addresses
  are decimal integers in the range 1-4294967295 (default: 1).

Streams can be opened in different *modes* depending on what connecting
clients plan -- or are allowed to -- do over them:

- Read (`r`): can read messages sent over the stream.
- Write (`w`): can write messages to the stream.
- Emit (`e`): can emit signals.

### Architecture

Transport proxies. Behaviour instances.

### Signals

### Behaviors

Behaviors are one of the pillarstones of Hydna and can be used to set up
rules for how Hydna should behave when streams are opened or when signals
are dispatched. While the most typical use-cases can be run without involving
behaviours, they can be used to unlock great control and enhancement.

Every account on Hydna has a behaviour-instance associated with it.

Standard library. Plugins.

Behaviors are powerful. No need to write a server. Can be used for logging,
authentication, backend plugins etc.

### Transports

A transport is a means of transfering data over Internet. There are a few
different transports clients can use to exchange messages over Hydna; each
with their individual traits:

- Hermes Binary Protocol (fast, not supported in browsers without using Flash)
- Web Sockets (fast, not supported by all browsers and devices)
- HTTP Longpolling (slightly slower but compatible with almost anything)
- HTTP REST (limited to pushing, but extremly easy to implement without a
  client library)

Which transport layer is used will be transparent to most developers (if you
use a client library, the choice of transport has already been made), but it's
an important architectural feature that developers should be aware of.

The characteristics of the environment determines which transport is most
suitable (WebSockets are great and can use a Flash-fallback, but neither works
in popular Apple devices. Longpolling works with almost any HTTP agent, but is
not as efficient as using Web Sockets) and it's up to the client library
developer to choose the right transport. In some cases multiple transports
might be needed -- the JavaScript implementation, for instance, automatically
detects if it should use native Web Sockets, Flash or HTTP Longpolling.
