---
title: The editor
order: 311
description: Changing content files and message lines in game, and the file watcher.
icon: pencil
---

```hocon
editor { enabled = true, watch-seconds = 3 }
```

On, content can be made and changed in game: `/skillsedit` and its windows, and a watcher that reloads a file within
`watch-seconds` of a save. Off, for a server that keeps its content in version control: no command, no input listener
and no watcher; every file still reads the same.

The [editor window](../menus/editor.md) walks folders, files and keys; a click asks for a new value (an anvil, by
`text-input.conf`). The words window walks languages and lines; a click asks for the new wording (in chat). The same is
done by command: `files`, `keys`, `show`, `set`, `new`, `delete`, and `read` then `write` for a message line.

Related: [Editor commands](../commands/editor.md)
