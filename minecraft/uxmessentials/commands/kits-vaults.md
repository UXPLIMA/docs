---
title: Kits & Vaults
order: 990
---

## Kits & Vaults

Two "claimable content" commands live here. `/kit` hands out predefined item sets on a cooldown, and `/vault` gives every player DB-backed personal storage that survives world rollbacks. Both are Brigadier commands, so tab-completion works everywhere, and both can be renamed, re-aliased, or disabled per install in [`commands/commands.conf`](../config/commands-conf.md). Permission nodes stay keyed to each command's stable id, so they never change when you rename a command.

---

## Kits

Single root `/kit` (default aliases `k`, `kits`). Running it bare opens the browse GUI, where players claim kits, preview contents, and see remaining cooldowns at a glance.

| Command | Description | Permission |
|---------|-------------|------------|
| `/kit` · `/kit list` | Open / list claimable kits | `uxmessentials.kit.use` |
| `/kit <name>` | Claim a kit | `uxmessentials.kit.use` |
| `/kit <name> <player>` | Give a kit to another player | `uxmessentials.kit.others` |
| `/kit show <name>` | Preview a kit without claiming it | `uxmessentials.kit.preview` |
| `/kit create` · `/kit del` · `/kit editor` | Define / delete / edit kit contents | `uxmessentials.kit.edit` |
| `/kit reset <player> [kit]` | Clear a player's claim / cooldown stamps | `uxmessentials.kit.reset` |

### Per-kit access & cooldowns

Beyond `uxmessentials.kit.use`, each kit carries its own gate node. A player must hold `uxmessentials.kit.<id>` — where `<id>` is the kit's name — to claim it.

| Node | Meaning |
|------|---------|
| `uxmessentials.kit.<id>` | Per-kit access gate (e.g. `uxmessentials.kit.starter`) |
| `uxmessentials.kit.cooldown.<seconds>` | Cooldown tier — the matching value replaces the kit's default wait |
| `uxmessentials.kit.cooldown.bypass` | Skip the cooldown entirely |

<Callout type="tip" title="Cooldown tiers stack with rank">

Grant `uxmessentials.kit.cooldown.30` to a donor rank and their `/kit` wait drops to 30 seconds without touching the kit definition. The `bypass` node always wins over any tier.

</Callout>

### Examples

```
/kit                     # open the browse GUI
/kit daily               # claim the "daily" kit
/kit starter Steve       # hand the starter kit to Steve
/kit show pvp            # preview the pvp kit's contents
/kit reset Steve daily   # let Steve claim "daily" again right now
```

---

## Vaults

Single root `/vault`. A player's vaults are stored in the database (never in the item — they survive world rollbacks). When a player owns several vaults, `/vault` opens a selector GUI; `/vault <n>` jumps straight to one.

| Command | Description | Permission |
|---------|-------------|------------|
| `/vault` · `/vault <n>` | Open the default vault, or vault N (selector when several exist) | `uxmessentials.vault.use` |
| `/vault info` | Show your vault count and quota limits | `uxmessentials.vault.use` |
| `/vault delete <n>` | Delete your own vault N (refunded if the economy is on) | `uxmessentials.vault.use` |
| `/vault rename <n> [name]` | Set or clear a vault's name | `uxmessentials.vault.rename` |
| `/vault icon <n> [material]` | Set or clear a vault's icon | `uxmessentials.vault.icon` |
| `/vault <player> [n]` | Open and audit another player's vault | `uxmessentials.vault.others` |
| `/vault delete <player> <n>` | Delete another player's vault (audited, no refund) | `uxmessentials.vault.admin.delete` |

### Quotas & marker nodes

How many vaults a player gets, and how large each one is, come from numbered quota nodes. The highest matching value wins.

| Node | Meaning |
|------|---------|
| `uxmessentials.vault.amount.<n>` | How many vaults the player may own |
| `uxmessentials.vault.size.<rows>` | Rows per vault (`<rows>` × 9 slots) |
| `uxmessentials.vault.free` | Waive the per-vault purchase cost |
| `uxmessentials.vault.bypass-blacklist` | Allow storing items the blacklist would normally reject |

<Callout type="warning" title="Blacklisted items">

Certain items can be barred from vaults in the module config. Players without `uxmessentials.vault.bypass-blacklist` cannot deposit them, and on overflow the plugin returns the item rather than deleting it.

</Callout>

### Examples

```
/vault                   # open your default vault (or the selector)
/vault 2                 # open your second vault
/vault info              # how many vaults do I have, and how big?
/vault rename 1 Ores     # name vault 1 "Ores"
/vault icon 1 diamond    # give vault 1 a diamond icon
/vault delete 3          # delete vault 3 (refunded if economy is on)
/vault Steve 2           # staff: inspect Steve's second vault
```

---

## Next Steps

- [🎒 Kits Feature Guide](../features/kits.md) — cooldowns, first-join kits, and the editor GUI
- [🗄️ Vaults Feature Guide](../features/vaults.md) — quotas, pricing, and blacklist setup
- [🔐 Permissions Reference](../permissions/reference.md) — full node list, including numbered quota tiers
- [⚙️ Renaming Commands](../config/commands-conf.md) — change any command name, alias, or toggle
