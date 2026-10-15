---
title: Configuration
order: 50
description: Every file the plugin writes, and what belongs in it.
icon: settings
---

The plugin writes its files into `plugins/uxmClaims/` on first start; the folder layout is shown in
[Installation](../getting-started/installation.md). Run `/claim reload` (permission
`uxmclaims.admin`) after editing a file; the few keys that need a restart are listed on their pages.

| Page | What it holds |
|---|---|
| [config.yml](config-yml.md) | Language, claim rules, integrations, dialogs, Bedrock forms, borders, blocks, holograms, database |
| [Languages](languages.md) | The eight shipped languages and what a switch rewrites |
| [roles.yml](roles-yml.md) | The roles every new claim starts with |
| [entitlements.yml](entitlements-yml.md) | Limits, costs, delays and cooldowns |
| [messages.yml](messages-yml.md) | Every message, its delivery type and sound |
| [webhooks.yml](webhooks-yml.md) | Discord embeds per claim event |
| [aliases.yml](aliases-yml.md) | Short commands mapped onto `/claim` subcommands |
| [Menu layouts](menus.md) | The chest menus, dialogs and Bedrock forms |

## How updates keep your edits

You never replace your files after an update. The plugin brings them forward itself, on startup only:

1. It reads the config version your files are at from `data/metadata.yml` (`version: 367` for 3.6.7).
   A folder without one counts as 3.4.4.
2. It creates every missing file from the jar, in the [installed language](languages.md).
3. It applies each release's patch newer than that version, oldest first, then records the new version.
4. It switches the language if `config.yml` asks for a different one.

A patch is a list of changes to specific keys, generated from the difference between two releases:

| Change | What it does to your file |
|---|---|
| Add | Writes a key the release introduced, with its default value |
| Delete | Removes a key the release dropped |
| Rename | Moves a key to its new path, value included |
| Modify | Changes a shipped default, but only where your value still equals the old default |

Every key and value a release did not change stays as you wrote it, comments included. Under a
language other than English, a patch writes that language's text, and English for any key the
translation lacks.

## Notes

- **Back up before a major update.** A Delete or Rename applies whether or not you edited the key:
  webhook blocks renamed in 3.6.7 are an example, see [webhooks.yml](webhooks-yml.md).
- **Do not edit or delete `data/metadata.yml`.** Without it the plugin assumes 3.4.4 and applies
  every patch again.
- **A file the plugin cannot parse stops it from loading.** Check the console after every edit; a
  stray tab or an unclosed quote is the usual cause.
