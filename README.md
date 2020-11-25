Moneysocket Relay Daemon
----

A straightforward websocket server that provides the rendezvous service for matching pairs of Moneysocket connections in rendezvous and forwarding encrypted messages between them.

"Decentralization"
------------------------------------------------------------------------

The concept of a socket is universal and does not particularly need any sort of server or cloud infrastructure to connect two devices. In the long view, Moneysocket is intended to work independently of particular socket types. In the short view, WebSockets are convenient to use as a basis for development and an easy way to get two devices connected.

Additional, web browsers typically only will want to connect to a server with a TLS certificate and typical end-users don't want to bypass their browser's security to enable WebSocket connections on their LAN.

This particular app is running at `wss://relay.socket.money` as a convenient way to rendezvous two connections, however this is at expense of the project ([donate](https://socket.money/#donate)). Also, it represents a single point of failure if it goes down and does not represent decentralization if this becomes critical infrastructure for production apps.

If your app needs relay services, please consider hosting your own relay app.

Also, we are *very very* interested in contributions that will adapt Moneysocket to other socket types (WebRTC, Bluetooth, NFC, etc) that have different properties when it comes to relying on server/cloud infrastructure.


End-to-End Encryption
------------------------------------------------------------------------

The way the Moneysocket protocol is defined, after the initial rendezvous, messages are encrypted using AES using the `shared_seed` value in the beacon which should only be known by the two devices at each end of the connection.

This prevents the relay app host from observing the contents of the messages.


Dependencies
------------------------------------------------------------------------

This depends on [py-moneysocket](https://github.com/moneysocket/py-moneysocket)

`$ pip3 install https://github.com/moneysocket/py-moneysocket`

to install the library and dependencies into your environment.


Running
------------------------------------------------------------------------

`$ ./relayd --config /path/to/config.conf`

To have the daemon listen on a low port (such as 443 which is the expected port for `wss://`), the `authbind` utility can be set up to enable that:

`$ authbind --deep ./relayd --config /path/to/config.conf`

For that to work, `authbind` must be set up with a file for the port in `/etc/authbind/byport/443` with the correct permissions for the user running `relayd`


Config
------------------------------------------------------------------------

A config file must be provided. Examples of TLS and non-TLS configurations are provided under [conf/](conf/). The config specifies the port, bind address and location to the certificate files.


Tips
------------------------------------------------------------------------

Using something like `nginx` or Amazon CloudFront etc. to front the WebSocket connection is probably advisable to manage connections and have some recourse against DDoS.


Project Links
------------------------------------------------------------------------

- [Homepage](https://socket.money).
- [Twitter](https://twitter.com/moneysocket)
- [Telegram](https://t.me/moneysocket)
- [Donate](https://socket.money/#donate)
