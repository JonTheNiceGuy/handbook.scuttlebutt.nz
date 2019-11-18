# ssb-server, ssb-client & Core Functions

## What is ssb-server?
[ssb-server](https://github.com/ssbc/ssb-server) is the component of secure-scuttlebutt that performs the peer-to-peer connections, and exposes via its API core functionality such as reading from, and writing to, the append-only log. It can also be extended with plugins, and expose their APIs alongside its own. An `ssb-server` process should be thought of as a sort of "background" process, since it can only perform the peer-to-peer "gossiping" functions if it is running. An `ssb-server` is currently one-to-one coupled with an "identity" which is essentially the secret key associated with a given append-only log.

> Sometimes, `ssb-server` is packaged as an "embedded" process with an SSB application, such as Patchwork. End users and developers can only safely run one application (with an embedded `ssb-server`) at a given time which interacts with your identity. You can stop one application and start another one, and that's fine. The SSB ecosystem is currently exploring a variety of solutions to this problem.

There are a variety of ways to access the API functions of `ssb-server`.

If the `ssb-server` process is started using the ssb-server command line command, it can be accessed:
- via a separate/second command line interface, using other `ssb-server` command line commands
- via `ssb-client` in a nodejs program, which connects to it via RPC (remote procedure calls) and has complete access to the API including extended functionality from plugins

If the ssb-server process is started programmatically, by importing it into a nodejs file, it can be accessed:
- directly via the in-program reference to the running server
- via a separate/second command line interface, using other `ssb-server` command line commands
- via `ssb-client` in a nodejs program, which connects to it via RPC (remote procedure calls) and has complete access to the API including extended functionality from plugins

## What is ssb-client?
[ssb-client](https://github.com/ssbc/ssb-client) is a nodejs module that can connect to a running `ssb-server` instance and expose its entire API, including that of the extended plugins, to a nodejs program. This, to an extent, resolves the concern over multiple `ssb-server` instances running, as one `ssb-server` process can be run, and one or more instances of `ssb-client` can connect to it. One challenge still being explored is that the `ssb-server` needs to be running all the right plugins for ALL the clients, which can present coordination challenges.

In many cases, `ssb-client` will be a good selection for interacting with a running `ssb-server` process.

## Accessing API functions

The following subarticles of this guide are divided according to core functionality of a running ssb-server process that you may wish to utilize, such as reading from, and publishing to, the feed (append-only log) for an idenity.

In each article, it is typical for there to be two references on how to access that function:

1. via the command line
2. via a reference in a program to the `ssb-server` API.

For the second one, this is written as `sbot`, and it is important to note that this `sbot` instance could be an original reference to a programmatically started `ssb-server` instance, or it could be an instance of an `ssb-client` connected to a running `ssb-server` process. Note that either way, with only minor differences*, the API is the same, and so we can call it a generic `sbot`, short for Scuttlebot.

> \*If you are directly interacting with ssb-server the "sync" methods are in fact synchronous (e.g. `var data = sbot.whoami()`), but if you're connecting over RPC, then "sync" methods are converted to "async" versions! (e.g. `sbot.whoami((err, data) => { ... })`)


