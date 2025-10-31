# Pear Architecture

Add intro that gives a high level overview of the entire architecture

## Bare Runtime

The [Pear Runtime](#pear-runtime) uses [Bare](../references/bare/overview.mdx) JavaScript runtime, which is a small and modular JavaScript runtime for desktop and mobile. Bare is a small modular Javasript runtime for both mobile and desktop apps.

Small and modular JavaScript runtime for desktop and mobile. Like Node.js, it provides an asynchronous, event-driven architecture for writing applications in the lingua franca of modern software. Unlike Node.js, it makes embedding and cross-device support core use cases, aiming to run just as well on your phone as on your laptop. The result is a runtime ideal for networked, peer-to-peer applications that can run on a wide selection of hardware.

Bare is built on top of [holepunchto/libjs](https://github.com/holepunchto/libjs), which provides low-level bindings to V8 in an engine independent manner, and [libuv/libuv](https://github.com/libuv/libuv), which provides an asynchronous I/O event loop. Bare itself only adds a few missing pieces on top to support a wider ecosystem of modules:

1. A module system supporting both CJS and ESM with bidirectional interoperability between the two.
2. A native addon system supporting both statically and dynamically linked addons.
3. Light-weight thread support with synchronous joins and shared array buffer support.

Everything else if left to userland modules to implement using these primitives, keeping the runtime itself succint and _bare_. By abstracting over both the underlying JavaScript engine using `libjs` and platform I/O operations using `libuv`, Bare allows module authors to implement native addons that can run on any JavaScript engine that implements the `libjs` ABI and any system that `libuv` supports.

:::info References

You can find implementation, usage and API details in the [Bare References](../references/bare/overview.mdx) section.

:::

## Pear Runtime

Though the Pear Runtime offers direct access to its base [API](../references/pear/api.md), in most cases you will start, publish and share your app through its [CLI](../references/pear/cli.md) and develop the app using its handy [building blocks](#building-blocks), [helpers](#helpers) and [tools](#tools).

:::info References

You can find implementation, usage and API details in the [Pear References](../references/pear/overview.mdx) section.

:::

## Building Blocks, Helpers and Tools

Explain the relation between these and the Pear runtime and themselves. 

### Building Blocks

The essential building blocks for building powerful P2P applications using [Pear](#pear-runtime).

:::info References

You can find implementation, usage and API details in the [Building Blocks References](../references/building-blocks/overview.mdx) section.

:::

#### Hypercore

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

#### Hyperbee

Hyperbee is an append only B-tree based on [`Hypercore`](#hypercore). It provides a key/value-store API, with methods for inserting and getting key-value pairs, atomic batch insertions, and creating sorted iterators. It uses a single Hypercore for storage, using a technique called embedded indexing. It provides features like cache warmup extension, efficient diffing, version control, sorted iteration, and sparse downloading.

:::info Single Writer

As with the Hypercore, a Hyperbee can only have a **single writer on a single machine**; the creator of the Hyperdrive is the only person who can modify it as they're the only one with the private key. That said, the writer can replicate to **many readers**, in a manner similar to BitTorrent.

:::

:::tip How To Use

Learn [how to share append only databases with Hyperbee](../how-to/replicate-and-persist-data/share-append-only-databases-with-hyperbee.md).

:::

#### Hyperdrive

Hyperdrive is a secure, real-time distributed file system designed for easy P2P file sharing. We use it extensively inside Holepunch; apps like Keet are distributed to users as Hyperdrives, as is the Holepunch platform itself.

Notable features include:

- Uses Hyperbee internally for storing file metadata
- Major API simplification. Instead of mirroring POSIX APIs, the new API better captures the core requirements of P2P file transfer.
- Auxiliary tools, [`localdrive`](../references/helpers/localdrive.md) and [`mirrordrive`](../references/helpers/mirrordrive.md), that streamline import/export flows and make it easy to mirror drives to and from the local filesystem.

:::tip How To Use

Learn [how to create a peer-to-peer filesystem with Hyperdrive](../how-to/replicate-and-persist-data/create-a-full-peer-to-peer-filesystem-with-hyperdrive.md).

:::

#### Autobase

Autobase is a multi-writer data structure for combining multiple writer cores into a view of the system. Using the event sourcing pattern, writers append blocks which are linearized into an eventually consistent order for building a view of the system, combining their inputs.

#### HyperDHT

The distributed hash table (DHT) powering [Hyperswarm](#hyperswarm) and built on top of [dht-rpc](https://github.com/holepunchto/dht-rpc). The [HyperDHT](../references/building-blocks/hyperdht.md) uses a series of holepunching techniques to ensure connectivity works on most networks and is mainly used to facilitate finding and connecting to peers using end-to-end encrypted Noise streams.

In the HyperDHT, peers are identified by a public key, not by an IP address. A public key can be connected regardless of where the peers are located, even if they move between different networks.

Notable features include:

- lower-level module provides direct access to the DHT for connecting peers using key pairs

:::tip How To Use

Learn [how to connect to two peers using HyperDHT](../how-to/connect-to-peers/connect-two-pears-with-hyper-dht.md).

:::

#### Hyperswarm

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

### Helpers

Helper modules can be used together with the building blocks to create cutting-edge P2P tools and applications.

:::info References

You can find implementation, usage and API details in the [Helpers References](../references/helpers/overview.mdx) section.

:::

#### Corestore

Corestore is a Hypercore factory that makes it easier to manage large collections of named Hypercores. It is designed to efficiently store and replicate multiple sets of interlinked [`Hypercore`](#hypercore)(s), such as those used by [`Hyperdrive`](#hyperdrive), removing the responsibility of managing custom storage/replication code from these higher-level modules.

:::tip How To Use

Learn [how to replicate and persist data with Hypercore](../how-to/replicate-and-persist-data/manage-multiple-hypercores-with-corestore.md).

:::

#### Localdrive

A file system API that is similar to [`Hyperdrive`](#hyperdrive). This tool comes in handy when mirroring files from user filesystem to a drive, and vice-versa.

#### Mirrordrive

Mirrors a [`Hyperdrive`](#hyerdrive) or a [`LocalDrive`](../helpers/localdrive.md) into another one.

#### Secretstream

SecretStream is used to securely create connections between two peers in Hyperswarm. It is powered by Noise and libsodium's SecretStream. SecretStream can be used as a standalone module to provide encrypted communication between two parties.

The SecretStream instance is a Duplex stream that supports usability as a normal stream for standard read/write operations. Furthermore, its payloads are encrypted with libsodium's SecretStream for secure transmission.

#### Compact-Encoding

A series of binary encoders/decoders for building small and fast parsers and serializers.

:::info Encodings

You can find the full list of bundled encodings in the [Compact Encoding References](../references/helpers/compact-encoding.md#bundled-encodings-bundled-encoding).

:::

#### Protomux

Multiplex multiple message-oriented protocols over a stream

### Tools

The following tools are used extensively employed in the day-to-day development and operation of applications built on Pear.

:::info References

You can find implementation, usage and API details in the [Pear References](../references/tools/overview.mdx) section.

:::

#### Hypershell

A command-line interface for generating and connecting to peer-to-peer, end-to-end encrypted shells.

#### Hypertele

A swiss-knife proxy powered by [HyperDHT](../references/building-blocks/hyperdht.md)!

#### Hyperbeam

An end-to-end encrypted pipeline for the Internet, utilizing the [`Hyperswarm`](../references/building-blocks/hyperswarm.md) and Noise Protocol for secure communications.

#### Hyperssh

A utility to facilitate SSH operations via the [HyperDHT](../references/building-blocks/hyperdht.md).

#### Drives

CLI to download, seed, and mirror a [Hyperdrive](#hyperdrive) or [Localdrive](#localdrive).
