# Lighthouse Peer

After [registration](./registration.md), and successful [authentication](./authentication.md), the peer opens a proxy on `127.0.0.1:42000`. This, along with all the following listeners are bound to `loopback` for obvious security reasons.

The peer proxy at `:42000` has the following endpoints

- `/coins` - Return the amount of coins left in the wallet
- `/proxy/:nickname/*` - Check for existing RTC connections listening. If an existing connection is not found, a WebRTC offer is made and sent to the [signaling](./signaling.md) server. A 20s timeout is used to prevent infinite waiting in case the other peer is offline. Every RTC connection has its own TCP tunnel. This endpoint redirects on that tunnel.
- `/stats` - It just return the amount of `goroutines`

The peer also starts a data server at `:42011` which is used in a TCP tunnel to send media. It also includes a file browser.

Ports `:42001` to `:42010` are reserved for RTC TCP tunnels where the peer consumes media. If all of these ports are used, the client rotates the TCP listener with the least used RTC connection.

## Over the wire security

All WebRTC communication is end-to-end encrypted using DTLS (Datagram TLS) (yes WebRTC uses UDP and UDP punch holes).

## TCP multiplexing over a single WebRTC Data Channel

Lighthouse uses a custom framing algorithm, along with custom multiplexing. As TCP is a stream oriented transport as opposed to WebRTC Data Channels, which are message oriented, the TCP stream needs to be broken up into frames. This is where some multiplexing magic can be inserted.

Every time a TCP listener accepts a connection, a new virtual stream is made. This stream has a unique ID, later used when re-assembling the frames. The stream is broken into (at max) 4kb long packets (else the channel will have an ahead-of-line data blocking problem). As this packet can be dynamic in size, an 8-byte header is attached to it. The header contains the stream ID (multiplexing magic), and the data size in little endian format. As data channels guarantee the order of delivery, the packets don't need a separate serial number, saving us from the TCP [SYN], [SYN, ACK]. Also it guarantees the validity and deliverability of a message, so no manual [ACK] stuff :)
