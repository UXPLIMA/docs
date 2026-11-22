---
title: Presets
order: 143
description: Ready-made module sets and values, applied from a command or the /uxm setup menu.
icon: layout-template
---

A preset writes `modules.yml` and a few module settings in one step, so a server can start as a towny-style
economy or a war server without editing files by hand. It never touches the license, storage settings or
webhook URLs: it can only write files under `modules/<id>/`.

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm preset list` | Lists the presets with their name and description | `uxmclaims.admin` |
| `/uxm preset show <preset>` | Shows what the preset would change, without changing anything | `uxmclaims.admin` |
| `/uxm preset apply <preset> [--dry-run]` | Shows the changes, then waits for `/uxm preset confirm`; with `--dry-run` it only shows them | `uxmclaims.admin` |
| `/uxm preset confirm` | Applies the preset shown by your last `apply`, within two minutes | `uxmclaims.admin` |
| `/uxm setup` | Opens the setup menu: pick a preset, click modules on or off, Apply. From the console it lists the presets | `uxmclaims.admin` |

## The shipped presets

Each shipped preset sets every module on or off. The table lists how each one differs from classic.

| Preset | Turns on | Turns off | Values it sets |
|---|---|---|---|
| `classic` | (the 3.x set: lands, expiry, invites, bans, warps, vault, claim-block, holograms, borders, notifications, land-chat, limits, economy, placeholders, webmap, webhooks, dialogs, bedrock, worldguard, luckperms, aliases, legacy-api) | every other module | `personal-lands.enabled: true` |
| `survival-claims` | | expiry, warps, vault, claim-block, holograms, land-chat, webhooks | `personal-lands.enabled: true` |
| `towny-economy` | areas, towns, nations, relations, bank, taxes, plots, levels, group-chat, ruins, audit-log, inbox, leaderboards | expiry, claim-block | No personal lands; wilderness denies `block_break`, `block_place`, `bucket_empty`, `ignite`; one town per player; town upkeep 10; bankruptcy on |
| `lands-style` | areas, nations, relations, bank, taxes, plots, levels, audit-log, inbox, leaderboards, claim-points | expiry, claim-block | Personal lands; flight; land banks with the owner's balance as fallback; land member tax, members who do not pay are removed |
| `war-server` | towns, nations, relations, bank, taxes, levels, group-chat, ruins, audit-log, newbie-protection, inbox, leaderboards, combat, war, rollback | expiry, warps, claim-block | No personal lands; PvP in the wilderness; enemies need not agree; 24h town join cooldown |
| `network` | network | vault | `personal-lands.enabled: true` |

`towny-economy`, `lands-style` and `war-server` expect Vault. `network` expects MySQL, MariaDB or PostgreSQL
storage, since the network module refuses to start on SQLite; restart every server after applying it.

## What applying does

1. The plan is worked out against the current files: module changes, value changes, and anything left out
   because this jar has no such module.
2. Every value is checked against the module's shipped file: a key the jar copy does not have, outside a
   section that takes free keys, is a problem and the preset is refused.
3. The files it changes are copied to `backups/preset-<id>-<time>/`.
4. `modules.yml` and the values are written with their comments kept.
5. When values changed, `/uxm reload` runs so settings that can change while running apply at once.
6. `data/setup.yml` records the preset as `last-preset` with `last-preset-at`.

Module changes apply after a restart. No land loses its protection when a module is switched.

Warnings do not stop a preset: a missing plugin, the wrong storage type, or a module whose dependency stays off
are shown with `!` and the preset still applies. Only problems (shown with `x`), such as an unreadable
`modules.yml` or an unknown key, refuse it.

## The setup menu and the first start

On a clean install (no `modules.yml`, no `data/state.yml`, no `data/setup.yml`), the plugin starts with the
modules' defaults, prints the list of presets to the console once and records `first-start` and
`hint-pending: true` in `data/setup.yml`. The first admin who joins gets a clickable line to `/uxm setup`, once.

`/uxm setup` opens two chest menus: the presets, then the modules of the one you picked with its choice
already set. Each click turns one module on or off; Apply writes the preset with your changes, with the same
backup and checks.

## Your own presets

Put a file in `plugins/uxmClaims/presets/<id>.yml`, with a kebab-case file name. A file with a shipped id
replaces that preset.

```yaml
config-version: 1
name: my-preset
requires-plugins: [Vault]
requires-storage: [mysql, mariadb, postgresql]
modules:
  towns: true
  expiry: false
set:
  - { file: modules/lands/config.yml, path: personal-lands.enabled, value: false }
  - { file: modules/lands/worlds.yml, path: [default, wilderness, "lands:pvp"], value: ALLOW }
```

Modules a preset does not list stay as they are. `path` is dotted, or a list when a key contains dots or colons.
Names and descriptions of the shipped presets are texts in `lang/<code>/messages.yml` under `preset.names`.

## Notes

- Values a module reads only at start-up need a restart, like the module switches.
- The setup menu has no Bedrock form yet.
- The `war-server` description still says "no war yet", but the preset turns `war` and `rollback` on.
- `lands-style` turns on nations and levels, which still need towns: on a server without towns they stay off
  with a warning.

Related: [modules.yml](modules-yml.md), [Modules](../modules/index.md), [Network mode](../network-mode.md)
