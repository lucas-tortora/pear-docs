# Building Blocks

The essential building blocks for building powerful P2P applications using [Pear](#pear-runtime).

:::info References

You can find implementation, usage and API details in the [Building Blocks References](../references/building-blocks/overview.mdx) section.

:::

## Hypercore

Hypercore is a secure, distributed append-only log built for sharing large datasets and streams of real-time data. It comes with a secure transport protocol, making it easy to build fast and scalable peer-to-peer applications.

Notable features include:

- Improved fork detection in the replication protocol, to improve resilience.
- Optional on-disk encryption for blocks (in addition to the existing transport encryption).
- A write-ahead log in the storage layer to ensure that power loss or unexpected shutdown cannot lead to data corruption.
- The [`session`](../references/building-blocks/hypercore.md#core.session-options) and [`snapshot`](../references/building-blocks/hypercore.md#core.snapshot-options) methods for providing multiple views over the same underlying Hypercore, which simplifies resource management.
- A [`truncate`](../references/building-blocks/hypercore.md#await-core.truncate-newlength-forkid) method for intentionally creating a new fork, starting at a given length. We use this method extensively in [`autobase`](../references/building-blocks/autobase.md).
 
:::tip How To Use

Learn [how to replicate and persist data with Hypercore](../how-to/replicate-and-persist-data/replicate-and-persist-with-hyper-core.md).

:::

## Hyperbee

Hyperbee is an append only B-tree based on [`Hypercore`](#hypercore). It provides a key/value-store API, with methods for inserting and getting key-value pairs, atomic batch insertions, and creating sorted iterators. It uses a single Hypercore for storage, using a technique called embedded indexing. It provides features like cache warmup extension, efficient diffing, version control, sorted iteration, and sparse downloading.

:::info Single Writer

As with the Hypercore, a Hyperbee can only have a **single writer on a single machine**; the creator of the Hyperdrive is the only person who can modify it as they're the only one with the private key. That said, the writer can replicate to **many readers**, in a manner similar to BitTorrent.

:::

:::tip How To Use

Learn [how to share append only databases with Hyperbee](../how-to/replicate-and-persist-data/share-append-only-databases-with-hyperbee.md).

:::

## Hyperdrive

Hyperdrive is a secure, real-time distributed file system designed for easy P2P file sharing. We use it extensively inside Holepunch; apps like Keet are distributed to users as Hyperdrives, as is the Holepunch platform itself.

Notable features include:

- Uses Hyperbee internally for storing file metadata
- Major API simplification. Instead of mirroring POSIX APIs, the new API better captures the core requirements of P2P file transfer.
- Auxiliary tools, [`localdrive`](../references/helpers/localdrive.md) and [`mirrordrive`](../references/helpers/mirrordrive.md), that streamline import/export flows and make it easy to mirror drives to and from the local filesystem.

:::tip How To Use

Learn [how to create a peer-to-peer filesystem with Hyperdrive](../how-to/replicate-and-persist-data/create-a-full-peer-to-peer-filesystem-with-hyperdrive.md).

:::

## Autobase

Autobase is a multi-writer data structure for combining multiple writer cores into a view of the system. Using the event sourcing pattern, writers append blocks which are linearized into an eventually consistent order for building a view of the system, combining their inputs.

## HyperDHT

The distributed hash table (DHT) powering [Hyperswarm](#hyperswarm) and built on top of [dht-rpc](https://github.com/holepunchto/dht-rpc). The [HyperDHT](../references/building-blocks/hyperdht.md) uses a series of holepunching techniques to ensure connectivity works on most networks and is mainly used to facilitate finding and connecting to peers using end-to-end encrypted Noise streams.

In the HyperDHT, peers are identified by a public key, not by an IP address. A public key can be connected regardless of where the peers are located, even if they move between different networks.

Notable features include:

- lower-level module provides direct access to the DHT for connecting peers using key pairs

:::tip How To Use

Learn [how to connect to two peers using HyperDHT](../how-to/connect-to-peers/connect-two-pears-with-hyper-dht.md).

:::

## Hyperswarm

Hyperswarm helps to find and connect to peers announcing a common 'topic' that can be anything. Using Hyperswarm, discover and connect peers with a shared interest over a distributed network. For example, we often use Hypercore's discovery key as the swarm topic for discovering peers to replicate with.

Hyperswarm offers a simple interface to abstract away the complexities of underlying modules such as [HyperDHT](../references/building-blocks/hyperdht.md) and [SecretStream](../references/helpers/secretstream.md). These modules can also be used independently for specialized tasks.

Notable features include:

- An improved UDP holepunching algorithm that uses arbitrary DHT nodes (optionally selected by the connecting peers) to proxy necessary metadata while being maximally privacy-preserving.
- A custom-built transport protocol, [UDX](https://github.com/holepunchto/libudx), that takes advantage of the holepunching algorithm to avoid unnecessary overhead (it doesn't include handshaking since holepunching takes care of that, for example). It's blazing fast.
- A simplified DHT API that closely resembles NodeJS's `net` module, but using public keys instead of IP addresses.
- 
:::tip How To Use

Learn [how to connect to multiple peers using HyperSwarm](../how-to/connect-to-peers/connect-two-pears-with-hyper-dht.md).

:::