---
title: Your first welcome
order: 104
description: Changing the welcome, turning it off for another plugin's greeting, and previewing a face.
icon: footprints
---

## 1. See a face

`/uxmheads preview <player>` prints that player's face as you would see it in chat.

## 2. Change the words

The welcome's lines are in `messages/messages_<language>.conf`:

```hocon
welcome {
  line-2 = "<face:2>  <h:'Welcome back'>"
  line-4 = "<face:4>  <body>Good to see you again, <value><player><body>."
}
```

Each line starts with its row; the words follow. Change them in every language file and run `/uxmheads reload`. A
first join reads `welcome.first.line-1` to `line-8`.

## 3. Or let another plugin greet

Set `welcome.enabled = false` and write `%uxmheads_face_1%` to `%uxmheads_face_8%` at the start of eight lines of that
plugin's join message.

## 4. Send it again

`/uxmheads welcome <player>` sends an online player the welcome again, to check a change.

Related: [The welcome](../features/welcome.md), [Placeholders](../placeholders/reference.md)
