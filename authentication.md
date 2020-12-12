# Authentication

To authenticate users, lighthouse does not use a traditional token based approach. Rather it uses cryptography to authenticate the users on each connection.

The server sends a 2048-bit long challenge (random string). As the peer possesses a `Curve25519` key-pair (see [registration](./registration.md)), the peer can use the private key to sign the challenge string. If the signature is verified using the public key, the peer is authenticated.
