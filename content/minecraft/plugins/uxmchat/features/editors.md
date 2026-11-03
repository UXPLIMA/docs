---
title: In-game editors
order: 418
description: Changing channels, tags and announcements in a window, with the changes reaching every server.
icon: pencil
---

Module: `editors`. Channels, tags and announcement bundles live in the database after the first start, and staff
change them in a window. A change reaches every server of a network.

| Command | Node | Edits |
|---|---|---|
| `/chateditor channels` | `uxmchat.editor.channels` | Every key of a channel |
| `/chateditor tags` | `uxmchat.editor.tags` | Every key of a tag |
| `/chateditor announcements` | `uxmchat.editor.announcements` | The bundles |
| `/chateditor announcement` | `uxmchat.editor.announcements` | Each announcement on its own |

## How it works

1. The [list window](../menus/editor.md) shows every object. A click opens it; the lime dye asks for an id and makes a
   new one.
2. The properties window shows one button per field. A text or number field asks for the new value, a switch flips, a
   choice steps to the next value. A blank answer removes the field, so the object falls back to its default.
3. A list, such as a tag's frames, a price's conditions or a bundle's announcements, opens in its own window: add a
   line, click one to change or remove it, go back with the feather.
4. The barrier deletes the object.

<Callout type="warning" title="Action lines can run console commands">

A tag's `on-success` and `on-failure` are action lines, and `[console]` runs a command as the console. Editing them
needs `uxmchat.editor.actions` as well as `uxmchat.editor.tags`. Give it only to people you would give the console.

</Callout>

Related: [Editor windows](../menus/editor.md), [Channels](channels.md), [Tags](tags.md), [Announcements](announcements.md)
