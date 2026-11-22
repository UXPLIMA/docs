---
title: Configuration
order: 140
description: The files uxmClaims 4.0 writes, where each one lives, and how updates keep your edits.
icon: settings
---

Everything lives in `plugins/uxmClaims/`. The kernel owns a few files at the top; each module owns a folder
under `modules/<id>/`. Module settings are documented on each module's page.

| Path | What it holds | Page |
|---|---|---|
| `config.yml` | Kernel settings: language, storage, license, protection, menus, migration | [Kernel config](kernel.md) |
| `modules.yml` | Which modules are on | [modules.yml](modules-yml.md) |
| `presets/<id>.yml` | Your own presets (the shipped ones are in the jar) | [Presets](presets.md) |
| `lang/<code>/messages.yml`, `commands.yml` | Kernel texts and `/uxm` command names | [Languages](languages.md) |
| `lang/<code>/menus/` | The setup menus, when you copy them out | [Menus](menus.md) |
| `theme.yml` | Colour tokens every text can use | [Languages](languages.md) |
| `modules/<id>/config.yml` and other `.yml` | One module's settings | the module's page |
| `modules/<id>/lang/<code>/` | One module's texts, command names and menus | [Languages](languages.md), [Menus](menus.md) |
| `data/` | `storage.db` (SQLite), `setup.yml`, `state.yml` (after a 3.x move), `shipped/` (records of shipped texts), license cache | [Database](database.md) |
| `backups/` | `config/` copies of rewritten files, `preset-<id>-<time>/`, `pre-4.0-<time>/` | |
| `migration/` | 3.x migration reports and state, `/uxm db` reports | [From 3.x](../upgrading/from-3x.md) |

## Reloading

`/uxm reload` (permission `uxmclaims.admin`) reads the kernel config, every module config, the theme, the
language files and the menu files again, closes open chest menus and lets each running module apply its new
settings. It does not start or stop modules, register commands or touch the database. Keys marked
`[restart]` in the file comments, `modules.yml` and `commands.yml` need a restart.

## How updates keep your edits

On start-up only, the config engine brings every file it manages up to date:

- Missing keys are added from the jar copy, with their comments (`config.auto-fill-missing-keys`).
- Keys the plugin does not know are reported and never deleted (`config.warn-unknown-keys`).
- A default that changed in the new version replaces your value only when you never changed it; values you
  edited are kept and listed once in the log (`config.update-unchanged-defaults`). The record of what was
  shipped lives in `data/shipped/`.
- Each file the plugin rewrites is copied to `backups/config/` first; the newest 20 copies are kept
  (`config.keep-backups`).
- Comments in your files are kept.

Language and menu files follow the same rule; `/uxm lang update` deals with texts the engine could not place,
see [Languages](languages.md). A file that does not parse is never written. On `/uxm reload` the previous good
copy stays in use. At start-up, a broken root `config.yml` stops the plugin, and a module whose config cannot
be read does not start. A single bad value falls back to its default on its own and is logged.

Related: [Kernel config](kernel.md), [modules.yml](modules-yml.md), [Database](database.md)
