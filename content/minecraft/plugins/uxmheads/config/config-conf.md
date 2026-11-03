---
title: config.conf
order: 501
description: Every key of config.conf, with its default and what it does.
icon: file-cog
---

## language

| Key | Default | Meaning |
|---|---|---|
| `language.default` | `"en"` | What a player reads when nothing else answers |
| `language.follow-client` | `true` | A player who chose nothing reads their client's language |
| `language.force` | `""` | One language for everybody |

## face

| Key | Default | Meaning |
|---|---|---|
| `face.glyph` | `"█"` | The character each pixel is drawn with. A narrower character draws a narrower face |
| `face.hat` | `true` | Draws the hat layer over the face |
| `face.fallback` | Steve | The skin of a player without one. Must be on a host in `skins.hosts`. Empty: no face |

## bedrock

| Key | Default | Meaning |
|---|---|---|
| `bedrock.face` | `"palette"` | `palette` or `hidden` |
| `bedrock.glyph` | `"█"` | The character on Bedrock |

## skins

| Key | Default | Meaning |
|---|---|---|
| `skins.hosts` | `["textures.minecraft.net"]` | The hosts a skin may be fetched from. Add your skin service's host |
| `skins.find-by-name` | `true` | Looks up the skin of a name on the account service |
| `skins.timeout` | `"5s"` | How long one download may take. Units `ms`, `s`, `m` |
| `skins.wait-at-login` | `"2s"` | How long a login waits for a skin never seen before. `0s` never waits |
| `skins.keep-days` | `30` | How long an unused skin file stays on disk |
| `skins.memory` | `1000` | How many faces are held in memory |

## welcome

| Key | Default | Meaning |
|---|---|---|
| `welcome.enabled` | `true` | Sends the welcome. The placeholders work either way |
| `welcome.delay` | `"500ms"` | How long after the join, once the face is ready |
| `welcome.first-join` | eight lines and a sound | The welcome of a first join, as effect lines |
| `welcome.join` | eight lines | The welcome of every other join |

Related: [Faces and skins](../features/faces-and-skins.md), [The welcome](../features/welcome.md)
