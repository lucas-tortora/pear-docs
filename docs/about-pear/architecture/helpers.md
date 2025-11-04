# Helpers

Helper modules can be used together with the [building blocks](building-blocks.md) to create cutting-edge P2P tools and applications.

:::info References

You can find implementation, usage and API details in the [Helpers References](../references/helpers/overview.mdx) section.

:::

## Corestore

Corestore is a Hypercore factory that makes it easier to manage large collections of named Hypercores. It is designed to efficiently store and replicate multiple sets of interlinked [`Hypercore`](#hypercore)(s), such as those used by [`Hyperdrive`](#hyperdrive), removing the responsibility of managing custom storage/replication code from these higher-level modules.

:::tip How To Use

Learn [how to replicate and persist data with Hypercore](../how-to/replicate-and-persist-data/manage-multiple-hypercores-with-corestore.md).

:::

## Localdrive

A file system API that is similar to [`Hyperdrive`](#hyperdrive). This tool comes in handy when mirroring files from user filesystem to a drive, and vice-versa.

## Mirrordrive

Mirrors a [`Hyperdrive`](#hyerdrive) or a [`LocalDrive`](../helpers/localdrive.md) into another one.

## Secretstream

SecretStream is used to securely create connections between two peers in Hyperswarm. It is powered by Noise and libsodium's SecretStream. SecretStream can be used as a standalone module to provide encrypted communication between two parties.

The SecretStream instance is a Duplex stream that supports usability as a normal stream for standard read/write operations. Furthermore, its payloads are encrypted with libsodium's SecretStream for secure transmission.

## Compact-Encoding

A series of binary encoders/decoders for building small and fast parsers and serializers.

:::info Encodings

You can find the full list of bundled encodings in the [Compact Encoding References](../references/helpers/compact-encoding.md#bundled-encodings-bundled-encoding).

:::

## Protomux

Multiplex multiple message-oriented protocols over a stream