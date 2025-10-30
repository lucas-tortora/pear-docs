---
slug: /
---
# Pear by Holepunch

> Pear loads applications remotely from peers and allows anyone to create and share applications with peers.

Pear by Holepunch is a combined Peer-to-Peer (P2P) Runtime, Development & Deployment tool.

Build, share & extend unstoppable, zero-infrastructure P2P applications for Desktop, Terminal & Mobile.

Welcome to the Internet of Peers

&nbsp; _– Holepunch, the P2P Company_

## Table of Contents

### Pear Runtime

References for Pear Runtime.

* [Command-Line-Interface (CLI)](./reference/pear/cli.md)
* [Application-Programming-Interface (API)](./reference/pear/api.md)
* [Application Configuration](./reference/pear/configuration.md)
* [Migration](./reference/pear/migration.md)
* [Troubleshooting Applications](./reference/pear/troubleshooting.md)
* [Frequently Asked Questions](./reference/pear/faq.md)

> The Pear Runtime uses [Bare](https://github.com/holepunchto/bare) JavaScript runtime, which is a small and modular JavaScript runtime for desktop and mobile. To learn more, see [Bare Reference](./reference/bare/overview.md).

### Guides

Guides on using the Pear Runtime to build and share P2P applications.

* [Getting Started](./guide/getting-started.md)
* [Starting a Pear Desktop Project](./guide/starting-a-pear-desktop-project.md)
* [Making a Pear Desktop Application](./guide/making-a-pear-desktop-app.md)
* [Starting a Pear Terminal Project](./guide/starting-a-pear-terminal-project.md)
* [Making a Pear Terminal Application](./guide/making-a-pear-terminal-app.md)
* [Sharing a Pear Application](./guide/sharing-a-pear-app.md)
* [Releasing a Pear Application](./guide/releasing-a-pear-app.md)
* [Making a Bare Mobile Application](./guide/making-a-bare-mobile-app.md)
* [Creating a Pear Init Template](./guide/creating-a-pear-init-template.md)
* [Best Practices](./guide/best-practices.md)


### How-tos

Simple How-tos on using the essential building blocks in Pear applications.

* [How to connect two peers by key with HyperDHT](./how-to/connect-two-peers-by-key-with-hyperdht.md)
* [How to connect to many peers by topic with Hyperswarm](./how-to/connect-to-many-peers-by-topic-with-hyperswarm.md)
* [How to replicate and persist with Hypercore](./how-to/replicate-and-persist-with-hypercore.md)
* [How to work with many Hypercores using Corestore](./how-to/work-with-many-hypercores-using-corestore.md)
* [How to share append-only databases with Hyperbee](./how-to/share-append-only-databases-with-hyperbee.md)
* [How to create a full peer-to-peer filesystem with Hyperdrive](./how-to/create-a-full-peer-to-peer-filesystem-with-hyperdrive.md)

### Building blocks

The essential building blocks for building powerful P2P applications using Pear.

| Name                                           | Description                                                                                                                          | Stability                                                 |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------- |
| [Hypercore](./building-blocks/hypercore.md)    | A distributed, secure append-only log for creating fast and scalable applications without a backend, as it is entirely P2P.          | **stable** |
| [Hyperbee](./building-blocks/hyperbee.md)      | An append-only B-tree running on a Hypercore. Allows sorted iteration and more.                                                      | **stable** |
| [Hyperdrive](./building-blocks/hyperdrive.md)  | A secure, real-time distributed file system that simplifies P2P file sharing and provides an efficient way to store and access data. | **stable** |
| [Autobase](./building-blocks/autobase.md)      | A "virtual Hypercore" layer over many Hypercores owned by many different peers.                                                      | **stable** |
| [Hyperdht](./building-blocks/hyperdht.md)      | The Distributed Hash Table (DHT) powering Hyperswarm.                                                                                | **stable** |
| [Hyperswarm](./building-blocks/hyperswarm.md)  | A high-level API for finding and connecting to peers who are interested in a "topic".                                                | **stable** |

### Helpers

Helper modules can be used together with the building blocks to create cutting-edge P2P tools and applications.

| Name                                             | Description                                                                                                                                                                 | Stability                                                 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| [Corestore](./helpers/corestore.md)              | A Hypercore factory designed to facilitate the management of sizable named Hypercore collections.                                                                           | **stable** |
| [Localdrive](./helpers/localdrive.md)            | A file system interoperable with Hyperdrive.                                                                                                                                | **stable** |
| [Mirrordrive](./helpers/mirrordrive.md)          | Mirror a [Hyperdrive](./building-blocks/hyperdrive.md) or a [Localdrive](./helpers/localdrive.md) into another one.                                                         | **stable** |
| [Secretstream](./helpers/secretstream.md)        | SecretStream is used to securely create connections between two peers in Hyperswarm.                                                                                        | **stable** |
| [Compact-encoding](./helpers/compact-encoding.md)| A series of binary encoding schemes for building fast and small parsers and serializers. We use this in Keet to store chat messages and in Hypercore's replication protocol.| **stable** |
| [Protomux](./helpers/protomux.md)                | Multiplex multiple message oriented protocols over a stream.                                                                                                                | **stable** |

### Tools

The following tools are used extensively employed in the day-to-day development and operation of applications built on Pear.

| Name                               | Description                                                                                                                   | Stability                                                 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| [Hypershell](./tools/hypershell.md)| A CLI to create and connect to P2P E2E encrypted shells.                                                                      | **stable** |
| [Hypertele](./tools/hypertele.md)  | A swiss-knife proxy powered by [HyperDHT](./building-blocks/hyperdht.md).                                                     | **stable** |
| [Hyperbeam](./tools/hyperbeam.md)  | A one-to-one and end-to-end encrypted internet pipe.                                                                          | **stable** |
| [Hyperssh](./tools/hyperssh.md)    | A CLI to run SSH over the [HyperDHT](./building-blocks/hyperdht.md).                                                          | **stable** |
| [Drives](./tools/drives.md)        | CLI to download, seed, and mirror a [Hyperdrive](./building-blocks/hyperdrive.md) or a [Localdrive](./helpers/localdrive.md). | **stable** |

### Apps

Applications built using Pear. 

- [Keet](./apps/keet.md): A peer-to-peer chat and video-conferencing application with end-to-end encryption.

### Examples

Collection of example applications that can be used as reference during development.
- [Bare on Mobile](./examples/bare-on-mobile.md): Reference applications for using Bare runtime on Android and iOS.
- [React App using Pear](./examples/react-app-using-pear.md): Example application for building Pear applications using React framework.

## Stability indexing

Throughout the documentation, indications of stability are provided. Some modules are well-established and used widely, making them highly unlikely to ever change. Other modules may be new, experimental, or known to have risks associated with their use.

The following stability indices have been used:

|                           Stability                          |                         Description                         |
| :----------------------------------------------------------: | :---------------------------------------------------------: |
|    **stable**   | Unlikely to change or be removed in the foreseeable future. |
| **experimental** |             New, untested, or have known issues.            |
| **deprecated** |           Being removed or replaced in the future.          |
|    **unstable**   |          May change or be removed without warning.          |
