---
title: config.conf
order: 601
description: Every key of config.conf, block by block, with its default and what it does.
icon: file-cog
---

The same file serves a Paper server and a Velocity proxy.

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `"en"` | What a viewer reads when nothing else answers |
| `follow-client` | `true` | A viewer reads the language their client reports. A server with one audience turns this off |
| `force` | `""` | One language for everybody; empty for none |

A file from before this block named the default as `locale = "en"`, and that is still read when the block has no default.

## motd

| Key | Default | Meaning |
|---|---|---|
| `rotation` | `"random"` | `in-order` or `random` |
| `entries` | two entries | A list of `{ icon, line-1, line-2 }`; `icon` is optional |

See [Entries](../features/entries.md).

## players

| Key | Default | Meaning |
|---|---|---|
| `mode` | `"just-one-more"` | `real`, `fixed` or `just-one-more` |
| `maximum` | `2026` | The maximum for `fixed` |
| `offset` | `0` | Added to the online count; may be negative |

## hover

A list of lines; see [The hover list](../features/hover.md).

## version

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `false` | Replace the protocol name with `text` |
| `text` | `"<good>● <b>1.21 - 26.2</b>"` | The replacement |
| `force-mismatch` | `false` | Send protocol -1 so the text always shows |

## centring

| Key | Default | Meaning |
|---|---|---|
| `box-width-pixels` | `267` | The width of the description box in font pixels |

## icons

| Key | Default | Meaning |
|---|---|---|
| `normal` | `"icons/uxm-mark.png"` | The icon of an entry that names none |
| `maintenance` | `"icons/maintenance.png"` | The icon while maintenance is on; not shipped |

## maintenance

| Key | Default | Meaning |
|---|---|---|
| `motd.line-1`, `line-2`, `icon` | two lines | The one entry shown while maintenance is on |
| `hover` | four lines | The hover while maintenance is on |
| `version.enabled`, `text`, `force-mismatch` | off | As `version`, for maintenance |
| `exempt.names` | `[]` | Names that may join, compared without case |
| `exempt.uuids` | `[]` | UUIDs that may join |

Related: [Configuration](index.md)
