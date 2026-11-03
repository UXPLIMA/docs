---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmCosmetics."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/cosmetics` | `/cosmetic`, `/wardrobe` | `open`, `boxes`, `equip`, `unequip`, `unequip all`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmcosmetics` | `/uxmcos`, `/cosmeticsadmin` | `grant`, `revoke`, `reload`, `cleanup`, `version`, `station add`, `station set`, `station show`, `station remove`, `station`, `station list`, `doctor` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmcosmetics.language` | Read this plugin in a language of your own. | true |
| `uxmcosmetics.admin.language` | Set or clear the language another player reads | op |
| `uxmcosmetics.use` | Open the wardrobe and wear what you are granted | true |
| `uxmcosmetics.*` | Every cosmetic, including the ones an operator adds later | op |
| `uxmcosmetics.box.*` | Every mystery box, including the ones an operator adds later | op |
| `uxmcosmetics.admin.*` | Everything an operator does with this plugin | op |
| `uxmcosmetics.admin.grant` | Give a player a cosmetic | op |
| `uxmcosmetics.admin.revoke` | Take a cosmetic off a player | op |
| `uxmcosmetics.admin.station` | Place and remove the wardrobe stations in the world | op |
| `uxmcosmetics.admin.cleanup` | Remove the entities a cosmetic left behind | op |
| `uxmcosmetics.admin.doctor` | Ask how this plugin is | op |
| `uxmcosmetics.admin.version` | Read the version this server runs | op |
| `uxmcosmetics.admin.reload` | Read the configuration and the cosmetics again | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmCosmetics/` on first start and reads them again on reload:

- `boxes.conf`
- `categories/balloon.conf`
- `categories/banner.conf`
- `categories/cloak.conf`
- `categories/emote.conf`
- `categories/gadget.conf`
- `categories/halo.conf`
- `categories/hat.conf`
- `categories/morph.conf`
- `categories/mount.conf`
- `categories/pet.conf`
- `categories/suit.conf`
- `categories/trail.conf`
- `commands.conf`
- `config.conf`
- `menus/boxes.conf`
- `menus/category.conf`
- `menus/roulette.conf`
- `menus/wardrobe.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `stations.conf`
- `theme.conf`
