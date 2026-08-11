---
title: Vaults
order: 1270
description: Vaults are personal, chest-style item storage that each player carries
  with them. A player can own several vaults, name and re-icon them, and open them
  from anywhere.
---

The defining property: vault contents are **stored in the database**, not on the
player. They survive relogs, server restarts, and (critically) **world rollbacks**.
Vaults are never PDC-backed.

The whole feature lives in the **`vaults`** module.

---

## How Vaults Work

Each player owns a numbered set of vaults (vault 1, vault 2, …). Two quotas shape
what they get:

| Quota | Controls |
|-------|----------|
| **Vault count** | How many separate vaults a player may own |
| **Vault size** | How many rows (of 9 slots) each vault has |

Both are granted through numbered permission nodes (below), so a rank can be given
more vaults and bigger vaults simply by adding permissions.

`/vault` opens your default vault. With several vaults, `/vault` opens a **selector
GUI**: a grid of your vaults (each showing its name and icon) that you click to
open. `/vault <n>` jumps straight to vault *n*.

<Callout type="info" title="DB-backed, rollback-safe">

Vault contents are written to the database (SQLite by default, or your networked
MySQL/MariaDB/PostgreSQL). Rolling back a world's region files does **not** touch
vault contents: this is the whole point of vaults over a placed chest.

</Callout>

---

## Commands

`/vault` is the single root. Vault numbers are 1-based.

| Command | What it does | Permission |
|---------|--------------|------------|
| `/vault` · `/vault <n>` | Open your default / Nth vault (selector when you have several) | `uxmessentials.vault.use` |
| `/vault info` | Show your vault count and quota limits | `uxmessentials.vault.use` |
| `/vault delete <n>` | Delete your own vault N (refund if economy is on) | `uxmessentials.vault.use` |
| `/vault rename <n> [name]` | Set / clear your vault's name | `uxmessentials.vault.rename` |
| `/vault icon <n> [material]` | Set / clear your vault's icon | `uxmessentials.vault.icon` |
| `/vault <player> [n]` | Open **and audit** another player's vault | `uxmessentials.vault.others` |
| `/vault delete <player> <n>` | Delete another player's vault (audited, no refund) | `uxmessentials.vault.admin.delete` |

---

## Naming & Icons

Players personalise a vault with `/vault rename <n> <name>` and
`/vault icon <n> <material>`; passing no argument clears the value. Both are gated by
their own nodes so you can, for example, let everyone rename but restrict custom
icons to donors. The bounds live in the config's `appearance` block
(`max-name-length`, `allow-custom-icon`).

---

## Economy & Deletion

If the vault module's economy block is enabled, vaults can cost money:

- **`cost-to-create`**: charged when a player first opens a vault slot they don't own yet.
- **`cost-to-open`**: a per-open charge (usually left at zero).
- **`refund-on-delete`**: when a player runs `/vault delete <n>`, the create cost is refunded.

Deleting **another player's** vault (`/vault delete <player> <n>`) is an admin action:
it is audited and **never** refunds.

<Callout type="note" title="Free access for staff">

Granting `uxmessentials.vault.free` lets a player use vaults without paying any
create/open cost, regardless of the economy settings.

</Callout>

---

## Blacklist & Overflow Rescue

Two safeguards protect the system:

- **Material blacklist**: items listed in `blacklist-materials` cannot be stored in a vault. Attempting to deposit one is blocked (holders of `uxmessentials.vault.bypass-blacklist` are exempt).
- **Overflow rescue**: if a player's effective vault *size* shrinks (a rank change, a config edit) so that stored items no longer fit, those items aren't silently deleted; they are rescued back to the player rather than lost.

---

## Admin & Audit Access

`/vault <player> [n]` opens another player's vault for inspection, and the access is
recorded to the audit log, so opening a player's vault is a traceable action, not a
silent peek. Deleting another player's vault is likewise audited.

| Node | Default | Grants |
|------|---------|--------|
| `uxmessentials.vault.others` | `op` | Open & audit any player's vault |
| `uxmessentials.vault.admin.delete` | `op` | Delete another player's vault |

---

## Permissions & Quotas

| Node | Meaning |
|------|---------|
| `uxmessentials.vault.use` | Use your own vaults (open, info, delete, defaults `true`) |
| `uxmessentials.vault.rename` | Rename your vaults |
| `uxmessentials.vault.icon` | Set a custom icon on your vaults |
| `uxmessentials.vault.amount.<n>` | **Quota:** own up to `<n>` vaults |
| `uxmessentials.vault.size.<rows>` | **Quota:** each vault has `<rows>` rows |
| `uxmessentials.vault.free` | Bypass all economy costs |
| `uxmessentials.vault.bypass-blacklist` | Store blacklisted materials |
| `uxmessentials.module.vaults` | Reload / inspect the vaults module |

The two `<n>`/`<rows>` nodes are open-ended numbered tiers: grant
`uxmessentials.vault.amount.5` and `uxmessentials.vault.size.6` to a rank and its
members get five vaults of six rows each. The module's `default-amount` and
`default-size` set the baseline before any permission tier applies.

---

## Key Settings

```hocon
# modules/vaults/config.conf
enabled = true
default-amount = 1        # vaults a player has before quota nodes
default-size = 6          # rows per vault before size nodes
open-sound = "..."        # sound played when a vault opens

cleanup {
  enabled = false         # prune vaults of long-inactive players
  inactive-days = 90
  interval-hours = 24
}

blacklist-materials = []   # materials that may never be stored
selector { }               # tuning for the multi-vault selector GUI
appearance { max-name-length = 32, allow-custom-icon = true }
economy { enabled = false, cost-to-create = 0, cost-to-open = 0, refund-on-delete = true }
```

---

## Tips & Gotchas

<Callout type="warning" title="EssentialsX has no vaults to import">

The EssentialsX importer brings homes, balances, warps, kits and more, but
EssentialsX stores no player-vault data, so **no vaults are produced** by a
migration. Vaults start empty.

</Callout>

- **Cleanup is off by default.** Turn on `cleanup.enabled` only if you want inactive players' vaults pruned; it's irreversible.
- Shrinking `default-size` or a rank's size tier triggers overflow rescue: plan quota reductions carefully.
- On a network, point every backend at the same MySQL/MariaDB/PostgreSQL database and vaults follow the player across servers.

---

## Next Steps

- [📟 Kits & Vaults Commands](../commands/kits-vaults.md) - Full command reference
- [🔐 Permission Reference](../permissions/reference.md) - Vault quota tiers and bypass nodes
- [⚙️ Per-Module Config](../config/per-module.md) - The `vaults` module block
- [💰 Economy](economy.md) - Enable paid vault creation and refunds
