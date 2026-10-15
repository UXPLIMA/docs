---
title: Languages
order: 501
description: The eight shipped languages, how to switch, and what a switch rewrites.
icon: languages
---

Every text the plugin shows ships in eight languages. One key in [config.yml](config-yml.md) picks the
language, and the plugin rewrites the files in your data folder to match it. File names and paths stay
the same in every language.

## The languages

| Code | Language |
|---|---|
| `en` | English (default) |
| `tr` | Türkçe |
| `es` | Español |
| `pt-BR` | Português (Brasil) |
| `de` | Deutsch |
| `fr` | Français |
| `ru` | Русский |
| `pl` | Polski |

Codes are case insensitive, `_` works in place of `-` (`pt_br`), and `pt` alone means `pt-BR`.

## Switching

```yaml
language: 'de'
```

Set the code in `config.yml`, then run `/claim reload` or restart. Before any file is read, the plugin
compares the code with the language the files are currently in and, if they differ:

1. copies every translated file to `backups/language-<old code>-<yyyyMMdd-HHmmss>/`, keeping the
   folder structure;
2. rewrites each file;
3. records the new language in `data/metadata.yml`;
4. logs how many texts changed and how many edited texts were kept.

A file is rewritten in one of two ways:

| The file | What happens |
|---|---|
| Untouched since it was written | Replaced whole by the new language's file, comments included |
| Edited | Each text still equal to the old language's default gets the new language's text; every text you changed stays as you wrote it |

A list, such as an item lore or the hologram lines, counts as one text: edit one line and the whole
list is kept. A key you deleted is not added back. `config.yml` is never replaced whole; only its texts
are rewritten.

If a file cannot be rewritten, the console says so and the new language is not recorded. The next
reload or restart tries again; files already switched are left as they are.

## What is translated

| File | Translated texts |
|---|---|
| `config.yml` | `claimSettings.defaultName`, the `blocks` icon names and lore, `hologram.lines` |
| `messages.yml` | Every message, the formats, `chatInputCancelWord`, `placeholderDefaults` |
| `roles.yml` | The names of the three system roles |
| `webhooks.yml` | Embed titles, descriptions and footers |
| `menu/*.yml` | All 28 menu files, including their dialog texts |
| `menu/bedrock/*.yml` | All 21 Bedrock form files |
| `entitlements.yml`, `aliases.yml` | Nothing: they hold no texts |

The translations sit in the plugin jar under `lang/<code>/`; the English files are the jar's root
files.

## Notes

- **Stored text does not change.** Claim names, role names and warp names already saved with a claim
  keep the language they were created in. Only new claims pick up the new role names.
- **An unknown code means English.** The console warns `Unknown language '<value>' in config.yml,
  using English` and lists the codes. A server running in German switches back to English on that
  reload. A `config.yml` without a `language` key also means English.
- **New files are created in the installed language.** A file you delete, or one a release adds, is
  written from the current language's translation, or from English where none exists. `config.yml` is
  the exception: it is always recreated from the English file, with `language: 'en'`, so set the code
  again after deleting it.
- **Do not edit `data/metadata.yml`.** It also holds the config version that
  [updates](index.md) rely on.
- **To undo a switch,** set `language` back to the old code and reload. The backup folder holds every
  file exactly as it was before the switch.

<Callout type="warning" title="Switch before you customise">

Texts you edit are never translated. Pick the language on a fresh install, then reword; switching
later leaves every edited text in the old language.

</Callout>

Related: [config.yml](config-yml.md), [messages.yml](messages-yml.md), [Menu layouts](menus.md)
