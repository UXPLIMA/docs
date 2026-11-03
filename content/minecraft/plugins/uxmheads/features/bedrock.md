---
title: Bedrock
order: 403
description: How a face is drawn for a Bedrock player.
icon: smartphone
---

A Bedrock client draws only the sixteen chat colours. Left to itself, the bridge picks the nearest of them by plain
numbers, and most skin tones then come out grey. uxmHeads picks the colour that looks nearest instead, so a face still
reads as a face.

| Key | Default | Meaning |
|---|---|---|
| `bedrock.face` | `palette` | `palette` draws the face in the sixteen chat colours; `hidden` draws none, and a row draws nothing |
| `bedrock.glyph` | `█` | The character on Bedrock, whose font is not the Java one |

Needs Geyser or Floodgate to tell a Bedrock player apart.

Related: [config.conf](../config/config-conf.md#bedrock)
