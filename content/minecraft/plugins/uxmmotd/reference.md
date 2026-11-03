---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmMOTD."
---

Contents: [Commands](#commands) · [Permissions](#permissions) · [Placeholders](#placeholders) ·
[Configuration](#configuration)

## Commands

The root is `/uxmmotd`, with the alias `motd`. Names, aliases, language-specific words and an on/off
switch for each word are in `commands.conf`; the table shows the shipped names.

| Command | Permission | What it does |
|---|---|---|
| `/uxmmotd` | `uxmmotd.admin.version` | Paper: prints a line pointing at `help`. |
| `/uxmmotd help [page]` | none | Paper: the uxmLib help page, listing only the words the sender may use. |
| `/uxmmotd version` | `uxmmotd.admin.version` | Prints the running version. |
| `/uxmmotd reload` | `uxmmotd.admin.reload` | Reads configuration, theme, messages and icons again, and says how long it took. |
| `/uxmmotd maintenance` | `uxmmotd.admin.maintenance` | Says whether maintenance is on. |
| `/uxmmotd maintenance on` | `uxmmotd.admin.maintenance` | Turns maintenance on. Says so when it was already on. |
| `/uxmmotd maintenance off` | `uxmmotd.admin.maintenance` | Turns maintenance off. Says so when it was already off. |
| `/uxmmotd maintenance status` | `uxmmotd.admin.maintenance` | Says whether maintenance is on. |
| `/uxmmotd doctor` | `uxmmotd.admin.doctor` | Runs the `content` and `icons` checks and prints one line each. |

## Permissions

| Permission | Default | Meaning |
|---|---|---|
| `uxmmotd.admin.version` | op | Read the version and the usage line of the bare command. |
| `uxmmotd.admin.reload` | op | Reload the files. |
| `uxmmotd.admin.maintenance` | op | Turn maintenance on and off and read it. |
| `uxmmotd.admin.doctor` | op | Run the doctor. |
| `uxmmotd.maintenance.bypass` | none | Velocity only: a player with it joins while maintenance is on. |

The four admin nodes are declared in `paper-plugin.yml` with the default `op`. Velocity has no defaults:
the console may use every command and a player needs the node from a permission plugin. The bypass node is
read on Velocity only and is not declared anywhere; grant it from a permission plugin.

## Placeholders

There is no PlaceholderAPI integration. Two tags work in every line of the server list entry (`motd`
lines, `hover`, `version.text` and their `maintenance` counterparts):

| Tag | Value |
|---|---|
| `<online>` | The online count this ping shows, after `players.offset`. |
| `<max>` | The maximum this ping shows, after `players.mode`. |

The message files take their own inserted values, each named in the line that uses it: `<version>` in
`admin.version`, `<ms>` in `admin.reloaded`, `<check>` and `<said>` in the doctor lines. A value inserted
this way is shown as written and never obeyed as markup.

## Configuration

Every key is in `config.conf` with its comment.

| Key | Default | Meaning |
|---|---|---|
| `language.default` | `en` | Language a viewer reads when nothing else answers. |
| `language.follow-client` | `true` | Read the language the client reports. |
| `language.force` | empty | One language for everybody. |
| `motd.rotation` | `random` | `in-order` or `random`. |
| `motd.entries` | two entries | List of `{ icon, line-1, line-2 }`; `icon` is optional. |
| `players.mode` | `just-one-more` | `real`, `fixed` or `just-one-more`. |
| `players.maximum` | `2026` | The maximum for `fixed`. |
| `players.offset` | `0` | Added to the online count. |
| `hover` | a list of lines | Lines on the player count hover. |
| `version.enabled` | `false` | Replace the protocol name with `version.text`. |
| `version.text` | a line | The replacement text. |
| `version.force-mismatch` | `false` | Send protocol -1 so the text always shows. |
| `centring.box-width-pixels` | `267` | Width of the description box in font pixels. |
| `icons.normal` | `icons/uxm-mark.png` | Icon used when an entry names none. |
| `icons.maintenance` | `icons/maintenance.png` | Icon used while maintenance is on, unless its entry names one. |
| `maintenance.motd` | two lines | The one entry shown while maintenance is on; `icon` is optional. |
| `maintenance.hover` | a list of lines | Hover lines while maintenance is on. |
| `maintenance.version` | disabled | Same keys as `version`, for maintenance. |
| `maintenance.exempt.names` | empty | Names that may join during maintenance. |
| `maintenance.exempt.uuids` | empty | UUIDs that may join during maintenance. |

`icons.maintenance` names a file the plugin does not ship; until you add it, the normal icon is used.
