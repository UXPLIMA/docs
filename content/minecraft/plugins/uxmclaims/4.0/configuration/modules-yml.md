---
title: modules.yml
order: 142
description: Turning modules on and off, the states /uxm modules shows, and how dependencies decide the start order.
icon: toggle-right
---

`plugins/uxmClaims/modules.yml` is the one place a module is turned on or off: one line per module id,
`true` or `false`. It is read at start-up, so every change needs a restart.

```yaml
config-version: 1
lands: true
towns: false
warps: true
```

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm modules` | Lists every module with its state, the plugin that provides it (for add-ons) and the reason it is not running | `uxmclaims.admin` |
| `/uxm reload` | Reloads config, language and menu files of the running modules; does not start or stop any | `uxmclaims.admin` |

## How the file is kept

| Situation | What happens |
|---|---|
| No `modules.yml` | Created with every module at its own default; on a clean install the [setup hint](presets.md) is shown once |
| A module the file does not list yet (a new version or add-on) | Added with the module's default and logged |
| An id no installed module provides | Kept and marked `# not found (the plugin or addon may have been removed)` |
| A value that is not `true` or `false` | Logged; the module's default is used |
| The file does not parse | Never rewritten; every module starts at its own default until you fix it |

Comments you write are kept. Turning a module off never deletes its data: its tables and its
`modules/<id>/` folder stay as they are, and it finds them again when you turn it back on.

A [preset](presets.md) writes this file for you, and `/uxm setup` lets you click modules on and off in a menu.

## Module states

| State | Meaning |
|---|---|
| `enabled` | Running |
| `disabled` | Turned off in `modules.yml` |
| `missing_dependency` | A module it depends on is off, missing or failed |
| `missing_plugin` | A plugin it needs, such as Vault, is not installed |
| `incompatible` | Written for an SDK level this uxmClaims does not support (too new or too old) |
| `unsupported_platform` | Not marked Folia safe on a Folia server |
| `failed` | Threw while loading or starting, its commands clashed with another's, its database migration failed or its config could not be read; whatever it registered was undone |
| `discovered`, `loaded` | Passing states during start-up |

A module that fails is turned off on its own; the others keep running.

## Dependencies and start order

Each module declares hard dependencies (it cannot run without them) and soft ones (it uses them when present).
At start-up the kernel decides, in this order:

1. Off in `modules.yml`: `disabled`. A required module (`lands`) starts anyway.
2. SDK level out of range: `incompatible`.
3. Folia without Folia support: `unsupported_platform`.
4. A required plugin missing: `missing_plugin`. A requirement such as `floodgate|Geyser-Spigot` is met by either.
5. A hard dependency that will not start: `missing_dependency`, passed down the chain.
6. The rest start in dependency order: hard and present soft dependencies first, required modules before
   optional ones, ties by id, so the order is the same on every start. Modules in a loop of hard dependencies
   fail; a loop of soft dependencies is broken and the modules start.

Turning on a module whose dependency is off leaves it at `missing_dependency`. Turn the dependency on too;
each module page names its requirements.

## Network mode

The `network` line is also read before the database opens, because it decides whether the database lease is
shared. See [Network mode](../network-mode.md).

## Notes

- No command switches a single module: edit the file, or use `/uxm setup`, then restart.
- `/uxm reload` takes no module name: it always reloads every running module's files.
- `/uxm modules` and `/uxm reload` answer with texts from `lang/<code>/messages.yml` (`modules`, `reload`).

Related: [Presets](presets.md), [Modules](../modules/index.md), [Kernel config](kernel.md)
