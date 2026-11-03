---
title: Storages
order: 402
description: The five storage types and the keys each takes.
icon: hard-drive
---

Each block under `storages` has an id, a `type` and `enabled`. A server may keep two of one type, such as an SFTP host
for the nightly copy and another at a second provider for the weekly. `config.conf` ships a commented example of each.
Any value may be `${NAME}`, read from the environment.

## local

| Key | Meaning |
|---|---|
| `folder` | Under the plugin's folder unless absolute: `"/mnt/backups"` is one line. Shipped `archives` |

## sftp

| Key | Meaning |
|---|---|
| `host`, `port` | The server; port 22 |
| `username`, `password` | The login |
| `key-file`, `key-passphrase` | A private key instead of a password, which is better |
| `known-hosts` | A `known_hosts` file; the host key is checked. Empty: it is not |
| `folder` | Where archives go on the host |
| `timeout-seconds` | How long a connection may take |

## s3

Any S3 endpoint: AWS, MinIO, Backblaze B2, Wasabi, Hetzner.

| Key | Meaning |
|---|---|
| `endpoint` | `https://s3.eu-central-1.amazonaws.com` or the provider's |
| `region` | The region |
| `bucket`, `prefix` | Where archives go |
| `access-key`, `secret-key` | The credentials |
| `path-style` | On by default for anything that is not Amazon, because a self-hosted endpoint has no bucket in its host name |

## webdav

| Key | Meaning |
|---|---|
| `url` | The folder's address, such as a Nextcloud `remote.php/dav/files/<user>/backups` |
| `username`, `password` | The login |

## ftps

| Key | Meaning |
|---|---|
| `host`, `port` | The server; port 21 |
| `username`, `password` | The login |
| `folder` | Where archives go |
| `implicit-tls` | `true` for implicit TLS; `false` for explicit |
| `passive` | Passive mode |

## Every storage

`keep-count`, `keep-size-mb` and `keep-days`: see [Retention](retention.md). `/uxmbackup storages` asks each whether it
answers and how much it holds. A run that reaches only some storages ends `partial`.

Related: [config.conf](../config/config-conf.md#storages), [Retention](retention.md)
