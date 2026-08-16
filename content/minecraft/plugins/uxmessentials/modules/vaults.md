---
title: Vaults
order: 9300
description: Personal chest-style storage kept in the database, so it survives a world rollback.
---

A vault is private, chest-style storage a player carries with them. `/vault` opens the default one, or a
selector grid when a player owns several, and `/vault <n>` jumps straight to one. Contents live in the database
rather than on the player, so rolling a world's region files back leaves a vault untouched.

Module `vaults` · enabled by default · `modules/vaults/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/vault` | Open one of your vaults, delete a vault, or audit another player's vault. | `uxmessentials.vault.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.vaults` | op | Hot-reload / inspect the vaults module (DB-persisted player vault storage). |
| `uxmessentials.vault.admin.delete` | op | /vault delete \<player> \<n>: delete another player's vault (audit-logged, no refund). |
| `uxmessentials.vault.amount.<n>` | everyone | How many vaults you may open; the largest tier held wins. |
| `uxmessentials.vault.bypass-blacklist` | op | Store any item in a vault, ignoring the blacklist-materials list (items are not returned). |
| `uxmessentials.vault.free` | off | Bypass every vault economy fee (create/open); no refund is paid on delete. |
| `uxmessentials.vault.icon` | everyone | /vault icon \<n> [material]: set or clear the icon of your own vault (held item if omitted). |
| `uxmessentials.vault.others` | op | /vault \<player> [n]: open and audit another player's vault (audit-logged). |
| `uxmessentials.vault.rename` | everyone | /vault rename \<n> [name]: set or clear the display name of your own vault. |
| `uxmessentials.vault.size.<rows>` | everyone | How many rows each of your vaults holds; the largest tier held wins. |
| `uxmessentials.vault.use` | everyone | /vault to open your default vault (or list them), /vault \<n> to open the Nth, and /vault delete \<n> to delete your own. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `default-amount` | `1` | vaults per player without a uxmessentials.vault.amount.\<n> node |
| `default-size` | `6` | rows per vault (1-6) without a uxmessentials.vault.size.\<n> node |
| `open-sound` | `""` | A sound played to a player when they open a vault. A Bukkit sound key (e.g. "block.chest.open" or the UPPER_SNAKE "BLOCK_CHEST_OPEN" form). Leave empty for no sound; an unknown name is silently ignored. |
| `cleanup.enabled` | `false` | run the inactive-vault purge sweep (false = vaults are never auto-deleted) |
| `cleanup.inactive-days` | `30` | a vault untouched for this many days is purged (minimum 1; 0 is treated as 1) |
| `cleanup.interval-hours` | `24` | how often the sweep runs (at least 1 hour) |
| `blacklist-materials` | `[]` | Materials a vault refuses to store. A blacklisted item placed in a vault is returned to the player when they close it (dropped at their feet if their inventory is full): it is never stored. Names are Bukkit material names, matched case-insensitively (e.g. "bedrock", "barrier"). The uxmessentials.vault.bypass-blacklist node lets staff store anything. Leave empty to allow every item. |
| `selector.enabled` | `true` | open the picker for a multi-vault owner (false = the old chat list) |
| `selector.rows` | `3` | picker menu height (1-6); the bottom row holds the page buttons |
| `selector.show-locked` | `true` | also show greyed icons for not-yet-unlocked indices up to the quota |
| `selector.owned-icon` | `"CHEST"` | icon material for an owned vault (unknown name falls back to CHEST) |
| `selector.locked-icon` | `"GRAY_STAINED_GLASS_PANE"` | icon material for a locked index |
| `appearance.max-name-length` | `32` | longest custom vault name accepted (1-256; a longer name is rejected) |
| `appearance.allow-custom-icon` | `true` | let players set a per-vault icon (false = /vault icon is refused) |
| `economy.enabled` | `false` | charge players for vault actions (requires an economy provider) |
| `economy.cost-to-create` | `0` | charged once when a new vault index is first allocated |
| `economy.cost-to-open` | `0` | charged each time a vault is opened (a per-open fee) |
| `economy.refund-on-delete` | `0` | paid back when the owner deletes their own vault |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_vaults_count%` | How many vaults the player holds. |
| `%uxmessentials_vaults_left%` | How many more vaults the player may open. |
| `%uxmessentials_vaults_max%` | How many vaults the player may open; the infinity marker when unlimited. |
| `%uxmessentials_vaults_size%` | How many rows each of the player's vaults holds. |
{/* /generated */}

## Notes

- **Two quotas shape what a player gets:** how many vaults they own and how many rows each one has. Both are
  numbered permission tiers, so a rank gets more and bigger vaults by holding a higher node.
- **Shrinking a quota does not eat items.** When a rank change or a config edit makes stored items no longer
  fit, they are handed back to the player instead of being dropped.
- **A vault opens in one window at a time.** If a staff member opens a vault its owner is using, or the other
  way round, the second opener is told to wait. Without that rule the later close would write back a stale copy.
- **Opening someone else's vault is audited,** as is deleting it. Staff access is a traceable action rather than
  a silent peek.
- **Costs are optional and refundable.** `cost-to-create` is charged the first time a player opens a slot they
  do not own, `refund-on-delete` gives it back when they delete their own vault, and deleting somebody else's
  vault never refunds. `uxmessentials.vault.free` waives the charges.
- **Blacklisted materials never enter a vault**, except for holders of the bypass node.
- **An EssentialsX import produces no vaults**, because EssentialsX stores none. Vaults start empty.
- **Cleanup is off by default and irreversible.** Switched on, it prunes inactive players' vaults.

Related: [Economy](economy.md), [Kits](kits.md), [Inventory Rollback](invrollback.md)
