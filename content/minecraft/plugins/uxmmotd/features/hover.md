---
title: The hover list
order: 404
description: The lines a client shows over the player count, and their limits.
icon: mouse-pointer
---

`hover` is the list a client shows when the cursor rests on the player count. The protocol carries those lines as player
names, not as text, so a client draws a handful of them and no more. A gradient does work: the line is sent as one colour
code per letter.

A blank line is a spacer, and a leading space indents a line under the title.

```hocon
hover = [
  "<b><g:'UXM Network':wheel></b>",
  "",
  " <icon>• <body>Gamemode: <good>Lobby hub",
  " <icon>• <body>Online: <value><online><body>/<value><max>",
  "",
  " <icon>→ <accent>Discord: <subtext><plain>ds.uxm.net</plain>",
  " <icon>→ <event>Store: <subtext><plain>store.uxm.net</plain>"
]
```

While maintenance is on, `maintenance.hover` is shown instead.

Related: [The player count](player-count.md), [Maintenance](maintenance.md)
