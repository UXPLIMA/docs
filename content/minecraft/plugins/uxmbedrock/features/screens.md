---
title: Screens
order: 1302
description: The player list, chat, scoreboard, titles, boss bars, name tags and holograms on Bedrock.
icon: monitor
---

| Switch | Screen | What a Bedrock player gets |
|---|---|---|
| `tab-list` | Player list | Java's names, colours and faces |
| `java-chat` | Chat | Narrow and low on the screen as on Java, on the PC layout |
| `scoreboard` | Sidebar | Polished lines; the score numbers Java does not show are hidden |
| `scoreboard-art` | Sidebar | The title's picture, and the background made see-through |
| `titles` | Titles, subtitles, action bar, boss bars | Polished text |
| `name-tags` | Name tags | Gradients, faces and pictures from whatever plugin sets them |
| `holograms` | Text displays | Polished text |

Bedrock has no tab key and no way to put the Java header and footer into its player list, so those two are not
drawn. See [What Bedrock cannot do](limits.md).

The changes to Bedrock's own screen files are the injection points of [gui.conf](../config/gui-conf.md). Each is a
best reading of Bedrock's screen files; when a Bedrock update moves an element, repoint it there.
