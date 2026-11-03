---
title: Maintenance
order: 407
description: Closing the server with one command, what the list shows, who still gets in, and the kick screen.
icon: construction
---

`/uxmmotd maintenance on` closes the server. While it is on:

| Part | Shows |
|---|---|
| The entry | `maintenance.motd`: its own two lines and optional icon |
| The hover | `maintenance.hover` |
| The version text | `maintenance.version`, off by default |
| The icon | `icons.maintenance`, unless the entry names one |
| A player joining | Refused with the kick screen `maintenance.kick` from the messages file, unless exempt |

`off` opens it again and `status` says which it is. The switch is saved in `maintenance.conf`, so a restart keeps it.

## Who still gets in

| Way | Platform |
|---|---|
| `maintenance.exempt.uuids`: a player's UUID | Both. It survives a name change, so prefer it |
| `maintenance.exempt.names`: a player's name, compared without case | Both |
| The permission `uxmmotd.maintenance.bypass` | Velocity only |

A Paper server checks the player before the player exists, and no Paper event that runs then carries a permission, so on
Paper the two lists are the only way.

## The kick screen

`maintenance.kick` in the messages files: "Maintenance. The server is closed for a short time. Please come back soon."
On Velocity a player reads it in their own language; on Paper it is written in the server's default language, since
there is no client yet.

## The shipped maintenance entry

```hocon
maintenance {
  motd {
    line-1 = "<center><b><g:'UXM Network':0></b> <icon>• <bad>Maintenance"
    line-2 = "<center><warn>We are working on the server. We are back shortly."
  }
}
```

Related: [Admin commands](../commands/admin.md), [config.conf](../config/config-conf.md#maintenance)
