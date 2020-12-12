# Registration

During registration, the client is asked the host of the [hub](https://github.com/lighthouse-p2p/hub), and a nickname. After both of them have been validated on the client, a new `Curve25519` key-pair is generated. The keys being binary are first encoded with base64, and then the public key, along with the nickname is sent to the hub.

After receiving them, the hub validates both of them (never trust client side data), and then stores them into a DHT (Distributed Hash Table).

After storing them, the hub asks the [blockchain](./blockchain.md) to add an initial coin amount to the wallet associated with that public key.
