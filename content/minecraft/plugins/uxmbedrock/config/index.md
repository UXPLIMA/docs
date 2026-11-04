---
title: Configuration
order: 150
description: Every file uxmBedrock reads, what each holds, and when a change takes effect.
icon: settings
---

All files are HOCON. `/uxmbedrock reload` reads them again and swaps the new settings in whole; a file that cannot be
read keeps the last good settings and says why in the console.

| File | Holds | Takes effect |
|---|---|---|
| [config.conf](config-conf.md) | Language, server name, storage, network, modules, player choices, preview page | Reload; `storage`, `network` and `preview` need a restart |
| [text.conf](text-conf.md) | Exact colours and the glyph pages | Reload, then the next build |
| [menus.conf](menus-conf.md) | Which menus of other plugins open as forms | Reload, at once |
| [pack.conf](pack-conf.md) | How the Bedrock pack is built | Reload, then the next build |
| [gui.conf](gui-conf.md) | Menu pictures and the changes to Bedrock's screens | Reload, then the next build |
| [items.conf](items-conf.md) | The vanilla item under an item model | Reload, then the next build |
| [commands.conf](commands-conf.md) | Command names, aliases and switches | Restart |
| [messages/](messages.md) | One file per [language](languages.md) | Reload |
| [theme.conf](theme.md) | The colours every UXM plugin shares | Reload |
| [Geyser extension](geyser-conf.md) | Where the extension finds the build | Geyser restart |
| [Velocity](velocity-conf.md) | The proxy plugin | Proxy restart |

When a release adds a key to config.conf, the next start adds it with its comment and never changes a value you
wrote; the file as it was goes to `backups/`, the last five copies. The other files are written once and never
changed by an update.
