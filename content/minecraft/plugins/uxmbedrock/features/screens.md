---
title: Screens
order: 1302
description: The player list, chat, scoreboard, titles, boss bars, name tags and holograms on Bedrock.
icon: monitor
---

| Switch | Screen | What a Bedrock player gets |
|---|---|---|
| `tab-list` | Player list | Tab shows Java's tab: header, footer, names, faces, ping and scores, laid out as on Java |
| `java-chat` | Chat | Narrow and low on the screen as on Java, on the PC layout |
| `scoreboard` | Sidebar | Polished lines, blank and repeated lines kept; the score numbers Java does not show are hidden |
| `scoreboard-art` | Sidebar | The title's picture, and the background made see-through |
| `titles` | Titles, subtitles, action bar, boss bars | Polished text |
| `name-tags` | Name tags | Gradients, faces and pictures from whatever plugin sets them |
| `holograms` | Text displays | Polished text |

## Java's tab

Bedrock's player list takes no header or footer, so the pack replaces it with a screen of its own. Pressing Tab (or
the touch button) shows what a Java player sees: the header, the players in Java's order (spectators last, then team
and name) with their team prefix and suffix, face, ping and list score, up to 20 a column, then the footer. Blank and
repeated header and footer lines are kept. Off, Bedrock players get Geyser's plain list.

## Blank sidebar lines

Bedrock drops a sidebar line that is empty and shows lines that read the same once. Each such line is sent with a
few spaces of its own, so every line of a Java sidebar shows, whether a plugin draws it with scores, teams or
packets.

The changes to Bedrock's own screen files are the injection points of [gui.conf](../config/gui-conf.md). Each is a
best reading of Bedrock's screen files; when a Bedrock update moves an element, repoint it there.
