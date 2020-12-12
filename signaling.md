# Signaling

As lighthouse uses WebRTC data channels to make a peer-to-peer TCP tunnel, the hub somehow needs to exchange the SDP (Session Descriptor Protocol) between peers.

Signaling uses websockets, and when the [authentication](./auth.md) is successful, it subscribes to a channel on [redis](https://redis.io) pub-sub.

The hub first checks the amount of coins before exchange "offer" type SDP. If the number is less than 1, the offer isn't forwarded to the other peer. "answer" type SDP is sill allowed, as the peer has no other way to earn coins than to serve some media.

First the SDP is `gzip`'d on the peer, and then is encoded using base64. This, along with a bit more info, is sent to the signaling server. The signaling server then uses redis pub-sub to publish the SDP on the other peer's channel.
