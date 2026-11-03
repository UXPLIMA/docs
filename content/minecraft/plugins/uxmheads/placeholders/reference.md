---
title: Reference
order: 801
description: Every %uxmheads_...% placeholder, relational faces, and how to lay a face beside words.
icon: list
---

PlaceholderAPI is required; the identifier is `uxmheads`. The answer is markup, not plain text, so it draws in a plugin
that reads a placeholder's answer as MiniMessage; every UXM plugin does. Each answer is read from memory: a face that
has not loaded yet answers the fallback, and the next ask answers the real one.

| Placeholder | Value |
|---|---|
| `%uxmheads_face_<row>%` | A row of the reader's face, the top one being 1 |
| `%uxmheads_face_<row>_<name>%` | A row of the face of the player called name, online or not |
| `%uxmheads_head%` | The small head the client draws itself, one line high |
| `%uxmheads_head_<name>%` | The small head of the player called name |
| `%rel_uxmheads_face_<row>%` | A row of the other player's face, for the reader |

A line written `%uxmheads_face_1% Welcome back` puts the words to the right of the first row. Eight lines, one per
row, draw the whole face.

## Examples

**Let another plugin greet players.** Set `welcome.enabled = false` and write `%uxmheads_face_1%` to
`%uxmheads_face_8%` into that plugin's join lines.

**A head beside a name in chat.** `%uxmheads_head% %player_name%` in a chat format draws the small head one line high.

Related: [Integrations](../integrations/overview.md)
