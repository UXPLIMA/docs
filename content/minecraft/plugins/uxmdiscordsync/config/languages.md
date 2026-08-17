---
title: Languages
order: 55
description: The Minecraft message files, and the Discord text that lives elsewhere.
icon: languages
---

Every message the plugin sends in Minecraft lives in `languages/<code>.yml`. Two ship (`en_US` and
`tr_TR`) and `plugin.language` picks one.

```yaml
plugin:
  language: "en_US"
localization:
  language: "en_US"
```

Both keys are read. Set them to the same value.

## The format

MiniMessage throughout, with a shared `prefix`:

```yaml
prefix: "<#00a2ff>uxmDiscordSync</#00a2ff> <dark_gray>►</dark_gray> "
no-permission: "<gradient:#FF6B6B:#C0392B>❌ You don't have permission to use this command!</gradient>"
cooldown: "<gradient:#FF6B6B:#C0392B>⏰ Please wait <yellow>{time}</yellow> seconds!</gradient>"
```

Message placeholders use `{braces}` (`{code}`, `{time}`, `{player}`) and are filled in by the
plugin. This is a different convention from the config files, which use `%percent%` for their
placeholders. When editing a message, keep whatever braces are already there.

Messages are grouped by feature: `linking`, `boost`, `rank-sync`, `two-factor` and so on.

## Adding a language

Copy `en_US.yml` to a new file, translate the values, and point `language` at the new name without
`.yml`.

Keep every key. A key you delete has no fallback: the message becomes empty rather than reverting to
English.

After a plugin update, compare your file against the shipped `en_US.yml`: new messages are written to
the shipped files only, and a missing key shows as nothing at all.

## What is not here

Discord-side text is not in the language file. Embeds, button labels, modal fields and slash command
descriptions are configured where the feature is:

| Text | Where |
|---|---|
| Link embed, button, modal | `config.yml → discord.linking` |
| 2FA embed and buttons | `two-factor.yml → embed`, `buttons` |
| Stats embed | `config.yml → server-stats.embed` |
| Log and audit embeds | `logging.yml` |
| Custom command replies | `discord-commands/*.yml` |
| Level-up DM | `level-system.yml → notifications` |

This is deliberate (those strings are part of a feature's configuration, not of a translation) but
it does mean translating the plugin fully means editing more than one file.
