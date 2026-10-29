---
title: config.yml
order: 52
description: The license, language, worlds, block behaviour and saving, with links to the larger sections.
icon: file-cog
---

`config.yml` holds the settings the whole plugin shares, the spawner types and the prices. Changes
apply with `/uxmspawners reload`.

## Top level

```yaml
license-key: ""
config:
  version: 4.2.0
  lang: en
database:
  type: SQLITE
```

| Setting | Meaning |
|---|---|
| `license-key` | Your license key. Without a valid one the plugin does not start |
| `config.version` | Written by the plugin to update your files when a new version is installed. Do not change it |
| `config.lang` | `en`, `tr`, `pl`, `de` or `fr`. The language of messages, menus and mob names. See [Language and Menus](languages.md) |
| `database.type` | `SQLITE` is the only one. Leave it |

## Worlds and blocks

| Setting | Default | Meaning |
|---|---|---|
| `disabled-worlds` | `[testWorld]` | Worlds where no spawner, vanilla ones included, can be placed or broken. Staff with `uxmspawners.bypassplace` can still place |
| `open-gui-on-click` | `true` | Right-clicking a spawner opens its menu. With `false` there is no way to open it, so storage cannot be emptied |
| `disable-xp-drops` | `false` | Spawner mobs give no XP, neither stored nor dropped |
| `disable-vanilla-spawners-spawning` | `false` | Vanilla spawners stop spawning anything |
| `protect-spawners-from-explosions` | `false` | Explosions leave uxmSpawners spawners alone. Off, an explosion destroys one with its storage |

## Mobs

| Section | Page |
|---|---|
| `use-modern-autokill` | [Auto-Kill](../features/auto-kill.md#modern-and-classic) |
| `nerf-spawner-mobs`, `no-equipment` | [Auto-Kill](../features/auto-kill.md#mobs-that-do-spawn) |

## Saving

```yaml
auto-save:
  interval: 300
  console-log: true
```

What spawners hold, their levels and their settings are written to `database.db` every `interval`
seconds, and when the server stops. A crash loses at most that long. Placing a spawner is written at
once.

`console-log: true` writes a line for each save. Keep it on: it is how you see that saving works,
and a failing save shows up there first.

## spawners and itemPrices

The spawner types are on [Spawner Types](spawner-types.md), the prices on [Prices](prices.md).

Related: [Configuration](index.md), [Installation](../getting-started/installation.md)
