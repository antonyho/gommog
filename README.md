# gommog

**GO** **M**assive **M**ultiplayer **O**nline **G**ame server is an online game server which aims to provide a generic MMO game server with high efficiency and to be able to host many users on a single instance.

The server design is inspired by the world first MMORPG - Ultima Online. And the server will be built on providing the feature and experience to act as a UO shard. But with the [Kaitai Struct](https://kaitai.io/), the server should be universally capable on hosting different kind of MMO game.

The design for the early versions will be based on a monolith server design. But later on, the server should be split into domain driven micro-services for scalability.


## Objectives

### Problems

There are multiple bottlenecks on some open source MMO game servers. They are mostly monolith. They don't have reliable game saving strategy with high speed. And game save process is usually not asynchronous. Otherwise, the efficiency isn't high and not dynamically scalable on processing power and network bandwidth. Many new online games are not like Ultima Online, they don't support as many players online in the same server. While OSI development team applied [partitioned world map to distributed game servers](https://youtu.be/zyVTxGpEO30), it has its own design problem especially when you walk through "server line". Other MMORPG like World of Warcraft employs other strategy, like [distributed realms](https://wowpedia.fandom.com/wiki/Connected_Realms).

### Solutions

#### Service Design

Domain driven services will be employed. They communicate through ordered message queues or event topics. Everything which affects any persisted attribute will be sent to the game save queue to be persisted. Fast storage is considered to be the persistent layer for the service.

#### Game Status Save

To use in-memory database for the cache and persistent. For example, Redis. So that we do not need to keep all status data in service's memory map.
MongoDB is also a good choice for the persistent layer. It is flexible and easy to manage for document structure.

#### Inter-services Communication

The design should be event driven and domain driven events communicate through message queues. Kafka is a good candidate.

#### AI

The possibility of using AI with machine learning trained model to drive the behaviours on the in-game NPC shall be explored.

### More Achievements

To let the server be universal for other MMORPG style online game. The network layer is kept isolated from the server core. So that custom client can be done easily.