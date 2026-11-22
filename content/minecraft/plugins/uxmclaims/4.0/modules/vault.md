---
title: Land Vault
order: 1307
description: A shared chest window that belongs to the land, not to a block.
icon: archive
---

Each land has a vault: a chest window its members open with `/claim vault` or the Vault button of the land menu. Only one player can have a vault page open at a time. The size and number of pages are set by the server.

Module `vault` · enabled by default · `modules/vault/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim vault [page]` | Opens the vault of the land you stand in | `uxmclaims.command.vault.open` and `uxmclaims.ability.vault.access` |

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.vault.open` | everyone | `/claim vault` |
| `uxmclaims.ability.vault.access` | everyone | Open land vaults where your role allows it; under LuckPerms it follows `undefined-ability` |
| `uxmclaims.admin.vault` | op | Open the vault of any land |

| Role permission | Default holder | Allows |
|---|---|---|
| `vault:access` (3.x `MANAGE_VAULT`) | Owner | Open the land's vault |

## Settings

| Key | Default | What it does |
|---|---|---|
| `rows` | `6` | Rows of the vault window (1 to 6) |
| `pages` | `1` | Pages per land vault |
| `blacklist` | `[]` | Materials that may not be stored, such as `PLAYER_HEAD`, `BUNDLE` |
| `on-land-delete` | `DELETE` | When a deleted land is purged: `DELETE` removes the items, `KEEP` leaves the rows in the database for staff |

With more than one page the last row holds the page buttons, so a page stores `(rows - 1) * 9` stacks; with one page it stores `rows * 9`.

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_vault_items%` | Stacks stored in the player's current land's vault |
| `%uxmclaims_vault_open%` | `true` when someone has that vault open |

## Notes

- Lowering `rows` or `pages` never deletes items: stacks beyond the new size stay saved and come back when you raise it again. The console warns at start when stacks are hidden this way.
- A page is saved when its window closes, when the player switches page or quits, and when the server stops.
- A deleted land keeps its vault while it can be restored; `on-land-delete` applies only when the land is purged.
- On a network, only the server the vault module is pinned to opens vaults; other servers tell the player so.
- Only members whose role has `vault:access` open the vault. Give it to the Member role in `roles.yml` if all members should share it.

Related: [Lands](lands.md), [Network](network.md)
