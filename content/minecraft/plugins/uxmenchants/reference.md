---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmEnchants."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/enchants` | `/enchantments`, `/ce` | `groups`, `info`, `list`, `limit`, `descriptions`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmenchants` | `/uxme`, `/enchantsadmin` | `version`, `doctor`, `reload`, `editor`, `apply`, `remove`, `give`, `giverandom`, `export`, `strip`, `import` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmenchants.use` | Open the windows and read what an enchantment does | true |
| `uxmenchants.enchantment.blunted` | Use the blunted enchantment | true |
| `uxmenchants.enchantment.curse-of-brittleness` | Use the curse of brittleness enchantment | true |
| `uxmenchants.enchantment.cushioned` | Use the cushioned enchantment | true |
| `uxmenchants.enchantment.ember` | Use the ember enchantment | true |
| `uxmenchants.enchantment.prospector` | Use the prospector enchantment | true |
| `uxmenchants.enchantment.scholar` | Use the scholar enchantment | true |
| `uxmenchants.enchantment.sharpened` | Use the sharpened enchantment | true |
| `uxmenchants.enchantment.smelter` | Use the smelter enchantment | true |
| `uxmenchants.enchantment.steadfast` | Use the steadfast enchantment | true |
| `uxmenchants.enchantment.thunderstruck` | Use the thunderstruck enchantment | true |
| `uxmenchants.enchantment.tree-feller` | Use the tree feller enchantment | true |
| `uxmenchants.enchantment.vein-miner` | Use the vein miner enchantment | true |
| `uxmenchants.admin.version` | Read the version this server runs | op |
| `uxmenchants.admin.doctor` | Ask how this plugin is | op |
| `uxmenchants.admin.reload` | Read the configuration and the content again | op |
| `uxmenchants.admin.language` | Set or clear the language another player reads | op |
| `uxmenchants.admin.give` | Give a player an enchantment book | op |
| `uxmenchants.admin.apply` | Put an enchantment straight onto a held item | op |
| `uxmenchants.admin.remove` | Take one enchantment off a held item | op |
| `uxmenchants.admin.strip` | Take every enchantment of ours off a held item | op |
| `uxmenchants.admin.export` | Write the content out to a file | op |
| `uxmenchants.admin.import` | Read content in from another plugin | op |
| `uxmenchants.admin.editor` | Build and change enchantments from inside the game | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmEnchants/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `editor/new-enchantment.conf`
- `enchantments/blunted.conf`
- `enchantments/curse-of-brittleness.conf`
- `enchantments/cushioned.conf`
- `enchantments/ember.conf`
- `enchantments/prospector.conf`
- `enchantments/scholar.conf`
- `enchantments/sharpened.conf`
- `enchantments/smelter.conf`
- `enchantments/steadfast.conf`
- `enchantments/thunderstruck.conf`
- `enchantments/tree-feller.conf`
- `enchantments/vein-miner.conf`
- `menus/editor-enchant.conf`
- `menus/editor-list.conf`
- `menus/editor.conf`
- `menus/grouped.conf`
- `menus/info.conf`
- `menus/wiki.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `rarities.conf`
- `targets.conf`
- `theme.conf`
- `types.conf`
- `vanilla.conf`
