---
title: Commands
order: 1690
icon: terminal
---

Chunklock adds a single **informational** command. The actual unlocking is done by
**dropping items** while looking at a locked chunk, not by a command, see
[How It Works](how-it-works.md).

All island management (create, home, visit, team, warps, upgrades) uses the normal
uxmSkyblock commands, see [Commands Overview](../uxmskyblock/commands/overview.md).

---

## `/chunklock`

Aliases: `/cl`, `/chunk`.

| Sub-command | What it shows |
|-------------|---------------|
| `/chunklock status` | Your number of unlocked chunks and the lock you're currently facing |
| `/chunklock cost` | The item requirement of the locked chunk you are looking at |

```
/chunklock status
/chunklock cost
```

The "facing" target is the chunk you're **looking at**: aim at a locked chunk to
see what it costs.

---

## Unlocking (no command)

To unlock a chunk you don't type anything; you **drop the item**:

1. Look at the locked chunk.
2. Press **Q** (drop) with the required item.
3. Repeat until the cost is met; the chunk opens.

---

## Permissions

| Node | Purpose |
|------|---------|
| `chunklock.bypass` | Walk through locked chunks freely (staff) |

There are no other Bukkit permissions: team access is governed by uxmSkyblock
[roles](../uxmskyblock/protection/roles-permissions.md).

---

## Creating an Island

Use the core command:

```
/ada oluştur      (= /is create)
```

If you try to drop items at a locked chunk without an island, Chunklock points you
to create one first.
