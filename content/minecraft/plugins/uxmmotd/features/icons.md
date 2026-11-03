---
title: Icons
order: 306
description: The picture beside the entry, the shipped ones, sizes, fallbacks and the maintenance icon.
icon: image
---

An icon is a PNG of exactly 64 by 64 pixels in the plugin folder, named relative to it, such as `icons/uxm-mark.png`.

| Key | Default | Used |
|---|---|---|
| `icons.normal` | `icons/uxm-mark.png` | When an entry names no icon |
| `icons.maintenance` | `icons/maintenance.png` | While maintenance is on, unless its entry names one |
| an entry's `icon` | | For that entry |

The plugin writes the two shipped pictures, `uxm-mark.png` and `uxm-glow.png`, into `icons/` on the first run and never
touches them again, so your own file is safe. `icons/maintenance.png` is not shipped: until you add it, the normal icon
is used.

A file that is missing, unreadable or the wrong size is reported once in the console and the ping is answered with the
normal icon, because a list entry with no picture reads as a broken server. `/uxmmotd doctor` says how many of the named
icons could be read. With no icon named anywhere, a ping carries the server's own `server-icon.png`.

`/uxmmotd reload` reads the icons again.

Related: [Entries](entries.md), [Maintenance](maintenance.md)
