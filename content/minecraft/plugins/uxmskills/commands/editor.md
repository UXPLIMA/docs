---
title: Editor commands
order: 304
description: Every /skillsedit branch.
icon: pencil
---

Every branch needs `uxmskills.admin.edit`, and the command exists only while `editor.enabled` is true.

| Command | Does |
|---|---|
| `/skillsedit open` | Opens the [editor window](../menus/editor.md); players only |
| `/skillsedit files <folder>` | Lists the files a folder holds |
| `/skillsedit new <folder> <file>` | Makes a new file from the template in `editor/` |
| `/skillsedit delete <folder> <file>` | Takes one file away |
| `/skillsedit keys <folder> <file>` | Lists the keys a file holds |
| `/skillsedit show <folder> <file> <key>` | Shows one key |
| `/skillsedit set <folder> <file> <key> <value>` | Changes one key |
| `/skillsedit read <language> <key>` | Reads a message line before rewriting it; players only |
| `/skillsedit write <language> <key> <text>` | Rewrites a line that was read first; players only |

The folders are `skills`, `sources`, `abilities`, `stats`, `menus` and `resources`. New files come from the templates `editor/new-skills.conf`, `new-sources.conf`, `new-abilities.conf` and `new-stats.conf`.

Related: [The editor](../features/editor.md)
