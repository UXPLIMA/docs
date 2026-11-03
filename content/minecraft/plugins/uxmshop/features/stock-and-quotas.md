---
title: Stock and quotas
order: 404
description: Server stock that refills, per player allowances, and what is not guaranteed.
icon: package
---

A line may limit each direction; both limits ship off, and a line that names neither is unlimited and writes no
counter.

```hocon
stock {
  buy {
    server { enabled = true, capacity = 640, refill = 64, every = "1h" }
    player { enabled = true, cap = 64, every = "24h" }
  }
}
```

| Key | Default | Meaning |
|---|---|---|
| `server.capacity` | 64 | A stock shared by everybody, up to this |
| `server.refill`, `server.every` | 64, `1h` | How much comes back and how often |
| `player.cap`, `player.every` | 64, `24h` | How much one player may trade per window |

A trade has to pass every limit written. A window that closed while the server was down is closed in one step. The
counters are in the database, so servers sharing it share them. `/uxmshop resetstock` and `/uxmshop resetquota` put
them back.

The check and the write are two steps, so two players buying the last item in the same instant can both pass. The
overshoot is bounded by how many click at once, and the money is never at risk.

Related: [Sections and lines](sections-and-lines.md)
