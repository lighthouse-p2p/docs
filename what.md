# What is lighthouse?

Lighthouse is a peer-to-peer media chain. It uses WebRTC Data Channels to create multiplexed TCP tunnels to share media. It uses blockchain. Don't get why? Read the next section.

## How is it different from bittorrent?

Peer-to-peer networks only survive when peers, along with leaching, seed some data back to the network. There is no enforcement on maintaining a seed-leach ratio. Lighthouse solves this by using a concept of coins. Coins are worth the data here. To get coins, the peers can seed back some media to the network. They can use up those coins by consuming some media from the network. After registration, each user gets an initial 100 coins (worth 100mb of data). To earn more, they just seed back media.
