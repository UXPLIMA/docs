---
title: Migrating
order: 313
description: Reading JetsMinions and LitMinions data into uxmMinions, and what is not carried.
icon: arrow-right-left
---

`/uxmminions migrate <jetsminions|litminions> [source]` reads another plugin's files and writes our own rows. It never
writes to theirs: their data is left as it is, so you can go back.

| From | Reads by default |
|---|---|
| `jetsminions` | `plugins/JetsMinions/players.yml`, then `players.db` |
| `litminions` | `plugins/LitMinions/database.db` |

`source` names another file. Every migrated minion gets an identity of ours, keyed by the minion and not the block. A
block that already holds one of ours, and a type this server has no file for, are skipped, so running it twice is safe.
What a migration cannot carry is reported line by line, never dropped in silence:

| From | Not carried |
|---|---|
| JetsMinions | Its base64 blob, its hunger (a minion arrives unfuelled), its stats counter, and the stock it keeps in a chest or on the item |
| LitMinions | The stock in its own item serialisation, its hunger, its exp and stat counters, its skins, particles and hologram switches |

Give your types the names the old plugin used, then run it.

Related: [Admin commands](../commands/admin.md)
