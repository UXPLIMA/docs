---
title: Overview
order: 201
description: How the tree is shaped, what the two permission systems mean, and aliases.
icon: list-tree
---

Everything lives under one root, `/claim`, with six sub-roots below it.

```
/claim                  the smart root, see Claim commands
├── chunk    …          add and remove land
├── block    …          the visual claim block
├── invite   …          the invitation flow
├── member   …          roles and per-member permissions
├── role     …          create and configure roles
├── warp     …          teleport points
└── admin    …          staff only
```

## Two permissions per command

Almost every command is gated twice:

| Gate | Node | Question |
|---|---|---|
| **Ability** | `uxmclaims.ability.<action>` | May this player ever do this, anywhere? |
| **Role permission** | A `ClaimPermission` on their role | May they do it *in this claim*? |

Both must pass. `uxmclaims.admin` skips both, and so does a `uxmclaims.bypass.*` node for the matching
action. The full node list is in [Ability permissions](../permissions/abilities.md).

Only one command is gated by a plain Bukkit permission in `plugin.yml`: `/claim reload`, which needs
`uxmclaims.admin`.

## Which claim a command acts on

Commands that say "the current claim" resolve it in this order:

1. The claim selected with `/claim select`, if any.
2. The claim you are standing in.

If neither applies you get *"No claim selected. Use /claim select or stand in a claim."*
`/claim select` with no argument opens a picker menu; `/claim select <name>` picks by name and is what
makes remote administration possible.

## Confirmation

Destructive actions route through a confirmation step, then `/claim confirm` or `/claim cancel`:

| Action | Asks first |
|---|---|
| `/claim` creating a brand new claim | yes |
| `/claim delete` | yes |
| `/claim kick` | yes |
| `/claim transfer` | yes |
| Teleporting to a public warp in a PvP claim | yes |
| `/claim ban` | no |

`/claim ban` is deliberately immediate: it exists for the moment someone is already causing damage.

## Aliases

`aliases.yml` maps a short command onto a `/claim` subcommand. Four ship by default:

| Alias | Runs |
|---|---|
| `/accept <claim>` | `/claim invite accept` |
| `/deny <claim>` | `/claim invite deny` |
| `/unclaim` | `/claim delete` |
| `/untrust <player>` | `/claim kick` |

Tab completion follows the target command automatically. Add your own and run `/claim reload`. See
[aliases.yml](../config/aliases-yml.md).

<Callout type="warning" title="/deny points at a subcommand that is spelled reject">

The shipped alias maps `/deny` onto `claim invite deny`, but the registered subcommand is
`/claim invite reject`. Either change the alias target to `claim invite reject`, or drop `/deny` and
tell players to use `/claim invite reject`. `/accept`, `/unclaim` and `/untrust` are all correct.

</Callout>
