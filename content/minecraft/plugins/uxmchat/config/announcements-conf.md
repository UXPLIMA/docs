---
title: announcements.conf
order: 610
description: The bundles a new database starts with, and every key of a bundle and an announcement.
icon: megaphone
---

`modules/announcements/announcements.conf`, read once, on the first start, and again only if the bundles table is
empty. After that the bundles live in the database and are changed with `/chateditor announcements`.

```hocon
bundles {
  tips {
    interval-seconds = 300
    order = "sequential"
    announcements {
      channels {
        lines = [ "@announce.channels" ]
        surfaces = [ "chat" ]
        min-playtime-minutes = 5
      }
    }
  }
}
```

Every key of a bundle and of an announcement, the conditions and the centred lines are in
[Announcements](../features/announcements.md).

Related: [Announcements](../features/announcements.md)
