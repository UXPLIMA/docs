---
title: "Guide"
order: 10
description: "How uxmHeads works, how to set it up and what each file holds."
---

uxmHeads draws players' faces in chat: eight rows of coloured characters, one per chat line, taken from the
player's skin. It sends a welcome with the face when a player joins and gives every other plugin the rows through
PlaceholderAPI.

## Install

Put `uxmHeads-<version>.jar` in `plugins/` and start the server. The first start writes `config.conf`,
`commands.conf`, `theme.conf` and the message files into `plugins/uxmHeads/`. Joining players get the welcome at
once. `/uxmheads reload` reads the files again.

## Where a face comes from

A player's profile names the address of their skin. The plugin downloads it only from a host listed under
`skins.hosts` (`textures.minecraft.net` out of the box), because a player's own client can put any address there.
An address on another host, or one that is not `http` or `https`, is never fetched.

- A player whose profile names no skin wears the skin their name has on the account service while
  `skins.find-by-name` is on. That covers servers that let players in without an account.
- A skin that cannot be read, and a player with no skin when `find-by-name` is off, wear `face.fallback`.
- Downloaded skins are kept in `skins/` and read from disk next time. A kept file that cannot be read is
  downloaded again. A file nobody wears for `skins.keep-days` is deleted.
- The face is the 8 by 8 face square of the skin, with the hat layer drawn over it while `face.hat` is on. An old
  64 by 32 skin whose hat area is fully opaque counts as wearing no hat, because those skins filled it with a solid
  colour. A high resolution skin is averaged down to 8 by 8.

## Configuration

| Key | Default | Meaning |
|---|---|---|
| `face.glyph` | `"█"` | The character each pixel is drawn with. |
| `face.hat` | `true` | Draws the hat layer over the face. |
| `face.fallback` | Steve | The skin of a player without one. Must be on a host in `skins.hosts`. Empty: no face. |
| `bedrock.face` | `"palette"` | `palette` draws the face in the sixteen chat colours a Bedrock client has, picked by what looks nearest; `hidden` draws none. |
| `bedrock.glyph` | `"█"` | The character on Bedrock. |
| `skins.hosts` | `["textures.minecraft.net"]` | The hosts a skin may be fetched from. |
| `skins.find-by-name` | `true` | Looks up the skin of a name on the account service. |
| `skins.timeout` | `"5s"` | How long one download may take. |
| `skins.wait-at-login` | `"2s"` | How long a login waits for a skin never seen before. `0s` never waits. |
| `skins.keep-days` | `30` | How long an unused skin file stays on disk. |
| `skins.memory` | `1000` | How many faces are held in memory. |
| `welcome.enabled` | `true` | Sends the welcome. The placeholders work either way. |
| `welcome.delay` | `"500ms"` | How long after the join, once the face is ready. |
| `welcome.first-join`, `welcome.join` | eight lines and a sound | The welcome as effect lines. `<face:1>` to `<face:8>` are the rows, `<player>` the name. |

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmheads` |  | Draw players' faces in chat. |
| `/uxmheads preview <player>` | `uxmheads.admin.preview` | See a player's face as you would in chat. |
| `/uxmheads refresh <player>` | `uxmheads.admin.refresh` | Read a player's skin again. |
| `/uxmheads welcome <player>` | `uxmheads.admin.welcome` | Send a player the welcome again. |
| `/uxmheads reload` | `uxmheads.admin.reload` | Read the configuration and the content again. |
| `/uxmheads version` | `uxmheads.admin.version` | Read the version this server runs. |
| `/uxmheads doctor` | `uxmheads.admin.doctor` | Say how this plugin is, in one line per thing that can be wrong. |
| `/uxmheads lang` | `uxmheads.language` | Say what language this player reads, and what else this server has. Players only. |
| `/uxmheads lang set <language>` | `uxmheads.language` | Read this plugin in one language, whatever the client is set to. Players only. |
| `/uxmheads lang reset` | `uxmheads.language` | Follow the client again, as a player who never chose anything does. Players only. |
| `/uxmheads lang set-player <player> <language>` | `uxmheads.admin.language` | Set the language another player reads. |
| `/uxmheads lang reset-player <player>` | `uxmheads.admin.language` | Clear the language another player chose. |

All nodes and their defaults are in [reference.md](reference.md).

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmheads_face_<row>%` | A row of the reader's face, the top one being 1. |
| `%uxmheads_face_<row>_<name>%` | A row of the face of the player called name. |
| `%uxmheads_head%` | The small head the client draws itself, one line high. |
| `%uxmheads_head_<name>%` | The small head of the player called name. |
| `%rel_uxmheads_face_<row>%` | A row of the other player's face, for the reader. |

A line written `%uxmheads_face_1% Welcome back` puts the words to the right of the first row.

## Examples

**Let another plugin greet players.** Set `welcome.enabled = false` and write `%uxmheads_face_1%` to
`%uxmheads_face_8%` into that plugin's join lines.

**Skins from your own skin service.** Add its host to `skins.hosts`.

**No faces for Bedrock players.** Set `bedrock.face = "hidden"`.
