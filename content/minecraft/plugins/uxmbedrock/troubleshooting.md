---
title: Troubleshooting
order: 197
description: What to check when Bedrock players get no pack, see plain items, lose menus or pictures, or are dropped while loading.
icon: life-buoy
---

Start with `/uxmbedrock doctor`: one line per thing that can be wrong, each with what to do. `/uxmbedrock status`
says what runs where, and `/uxmbedrock pack report` what the last build found and left out. The whole report is in
`plugins/uxmBedrock/output/report.txt`.

A Bedrock client shows its own errors in its content log. Turn it on under **Settings > Creator > Content Log GUI**:
a missing texture or a broken screen file is named there.

## The plugin does not start

| The console or the doctor says | Fix |
|---|---|
| `uxmBedrock runs on Paper and Folia 26.2, 26.3 and 26.4` | Run one of those versions |
| `Floodgate is missing` | Install Floodgate on every Paper server |
| `This server's Java runs in ...` a Turkish or Azerbaijani locale | Start Java with `-Duser.language=en -Duser.country=US`; see [Installation](getting-started/installation.md#requirements) |
| A file has an error | The console names the file. Fix it and run `/uxmbedrock reload`; a failed reload keeps the last good settings |
| `network.mode is direct but network.secret is empty` | Copy this server's secret from the proxy's `secrets.conf`. The server runs alone until then |
| `TCPShield refuses Geyser's connection` | Set `enable-geyser-support: true` in `plugins/TCPShield/config.yml` and restart. TCPShield needs Floodgate for it |

## Bedrock players get no pack

| Cause | Fix |
|---|---|
| The extension is not installed | Put `uxmBedrock-Geyser-<version>.jar` in Geyser's `extensions/` and restart Geyser |
| `No uxmBedrock build in ... yet` in Geyser's console | The first build has not finished, or the extension looks in the wrong folder: set `source.folder` in its [config](config/geyser-conf.md) |
| `The Bedrock pack build failed` | The reason follows on the same line; `/uxmbedrock status` repeats it |
| A source is missing from the report | The plugin that builds it is not running, or protects its pack. The report names the setting to change; see [The pack](features/pack.md#sources) |
| A player declined the download | They rejoin and accept |

## Items, blocks or heads look vanilla

| Cause | Fix |
|---|---|
| Geyser has not loaded the new items yet | Geyser reads custom items, blocks, names and heads at its start. Restart it, or set [`new-items-restart`](config/pack-conf.md) to `when-empty` |
| The report says an item model needs a base item | `/uxmbedrock items unknown` lists them. Answer each in [items.conf](config/items-conf.md), or hold the item once as a Java player |
| A model is left out as unused | No item, block state or font names it. That is right: Java never shows it either |
| Furniture shows as a display, not a block | Bedrock has no block for it; see [What Bedrock cannot do](features/limits.md) |

## Players are dropped while the packs load

| Cause | Fix |
|---|---|
| A phone takes minutes to load a large pack, and the server times it out | Keep `loading-protection-seconds` above 0 in the [extension's config](config/geyser-conf.md). Geyser's console logs each load of 10 seconds or more |
| The pack is large | Turn on the [pack host](config/geyser-conf.md#pack-host), much faster than Geyser's own transfer. `split-by-content` keeps a change to one part from sending the rest again |

## Menus

| Problem | Cause and fix |
|---|---|
| A menu opens as a form and should be a chest, or the other way | Add a rule to [menus.conf](config/menus-conf.md). `/uxmbedrock menus report` lists every menu Bedrock players opened since the start and how each reached them |
| A menu's picture is missing | `menu-art` is off under `modules`, the letter is under `art-min-height` in gui.conf, or the player switched menu pictures off |
| A picture sits wrong | Measure it with `/uxmbedrock gui measure`, or place it in the [preview page](features/preview.md) |
| The words of a title stand somewhere else than on Java | Text Java draws back over earlier text stays after it on Bedrock; see [Menu pictures](features/menu-art.md#words-over-the-picture) |
| A chest does not open, and Geyser says `exceeded 7 attempts to open a virtual inventory` | Something covered the player's screen when the menu opened, such as the content log. Close it and open the menu again |

## A build went wrong

`/uxmbedrock pack builds` lists the builds the cache keeps, newest first, and marks the one players get.
`/uxmbedrock pack rollback` gives players the build before the newest again, from their next join, until the next
build. Turn `auto-convert` off in [pack.conf](config/pack-conf.md) while you fix the source, or its next change builds
again. See [Past builds](features/pack.md#past-builds).

## After a Bedrock update

What the pack draws in screens depends on Bedrock's own screen files. When an update moves an element, a menu
picture, the Java tab or the chat may be out of place. Repoint the injection point in [gui.conf](config/gui-conf.md),
or update uxmBedrock.
