---
title: Restoring and lapsing
order: 302
description: What happens to a glow at quit, at join, and when a rank expires.
icon: undo-2
---

The choice is stored in the database. At quit the outline is taken down; nothing is lost.

At join the stored choice is read and every colour in it is checked against the player's nodes again:

| Result | Moment |
|---|---|
| Every colour is still theirs | `effects.restored`: `glow.restored`, and the outline is back |
| A colour is no longer theirs | `effects.lapsed`: `glow.lapsed`, and the glow stays off |

A rank can expire between two sessions. Restoring without the check would let a player keep a colour they no longer
hold, silently. An offline player holds no permission the server can read, so a check that cannot be made refuses.

On a network sharing one database, `storage.sync-seconds` (2) is how often a server reads the choice another server
changed for a player who is here.

Related: [Moments](moments.md), [Database](../database/overview.md)
