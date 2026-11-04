---
title: pack.conf
order: 604
description: "Every key of pack.conf: sources, output, sounds and animation."
icon: package
---

| Key | Default | Meaning |
|---|---|---|
| `auto-convert` | `true` | Build again on its own when a source pack changes |
| `check-every-seconds` | `30` | How often the sources are looked at; at least 5 |
| `merge-bedrock-packs` | `[]` | Bedrock packs you already have, put in front of the converted one as they are, relative to the server folder |
| `name` | `"uxmBedrock"` | The pack's name. It also makes the pack's id: renaming it makes every player download it again |
| `description` | `"The server's own look"` | The line under the name in the Bedrock pack list |

## sources

| Key | Default | Meaning |
|---|---|---|
| `server-properties` | `true` | Fetch the pack `resource-pack` in server.properties names and convert it too |
| `extra` | `[]` | More packs, a zip or a folder each, relative to the server folder or absolute. Later packs win where two hold the same file |

## items

| Key | Default | Meaning |
|---|---|---|
| `icon-size` | `64` | The size of a flat item's icon in pixels, 16 to 256 |

## output

| Key | Default | Meaning |
|---|---|---|
| `png-compression` | `6` | 0 (fastest) to 9 (smallest) |
| `zip-compression` | `6` | 0 (none) to 9 (smallest) |
| `split-by-content` | `true` | Items, blocks, sounds and glyphs in packs of their own, so a change sends only the part that changed |
| `split-above-megabytes` | `20` | A part larger than this is split again, 1 to 1024 |

## sounds

| Key | Default | Meaning |
|---|---|---|
| `compress` | `true` | Re-encode sounds smaller with ffmpeg when it is installed; without it they are copied |
| `mono` | `false` | Make compressed sounds mono |

## animated

| Key | Default | Meaning |
|---|---|---|
| `skip` | `[]` | Textures whose animation is not carried over, as `namespace:path`; they keep their first frame |
