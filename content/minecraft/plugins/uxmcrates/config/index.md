---
title: Configuration
order: 50
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

The plugin writes its files into `plugins/uxmCrates/` on first start; the folder is shown in
[Installation](../getting-started/installation.md#the-files). Every file is HOCON. Run `/uxmcrates reload`
(`uxmcrates.admin.reload`) after an edit; the keys that need a restart say so on their page.

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, idle effects, odds, pushback, claims, log, placeholders, boards, ledger, broadcast, rerolls, milestones, luck, purge, retention, link tool, editor, window sounds |
| [opening.conf](opening-conf.md) | Who sees a reveal, skipping, the bulk cap, the reveal window panes, the preview wait |
| [economy.conf](economy-conf.md) | Treasury's wait, placeholder currencies, economy bridges |
| [Crate files](crate-files.md) | `crates/<id>.conf`: one crate each |
| [Reward files](reward-files.md) | `rewards/<id>.conf`: a reward shared by crates, and the item format |
| [Key files](key-files.md) | `keys/<id>.conf`: one key each |
| [Animation files](animation-files.md) | `animations/<id>.conf`: one reveal each |
| [Envoy files](envoy-files.md) | `envoys/<id>.conf`: one envoy each |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | `menus/`: every window |
| [Messages](messages.md) | `messages/messages_<language>.conf`: every line the plugin writes |
| [Languages](languages.md) | The seven languages, adding one, and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## HOCON in two minutes

| Rule | Example |
|---|---|
| A block opens and closes with braces | `odds { show = true }` |
| `=` or `:` between key and value; a block needs neither | `velocity = 0.4`, `idle { ... }` |
| Text in quotes, numbers and `true`/`false` without | `animation = "reel"`, `no-repeat = true` |
| Lists in square brackets, commas or new lines between | `items = ["DIAMOND 3"]` |
| A comment starts with `#` | `# 0 is no wait` |
| A key with dots or digits first is quoted | `"uxmcrates.luck.tier-one" = 1.25`, `"10" { ... }` |

## A line that does not read

In a content file (a crate, key, reward, envoy or animation), a line the plugin cannot read is refused and named in the
console with its file and key, and the rest of the file still loads. `/uxmcrates doctor` counts the refused lines. A
crate that names an economy the server does not run, or a key that does not exist, is not loaded at all.

## Reloading

`/uxmcrates reload` reads `config.conf`, `opening.conf`, `economy.conf`, every crate, key, reward, animation and envoy
file, the key recipes, the windows, the messages and the menu sounds again, and swaps them in one go. A file that does
not parse stops the reload with `admin.reload-failed`, and the server keeps what worked.

| Not read by a reload | Why |
|---|---|
| `commands.conf` | The server builds its command tree once, at start |
| `storage` | The database is opened once, at start |
| `leaderboard.refresh-seconds` | The board timer is made at start |

## What an update does

You never replace your files after an update. On the start after a new jar:

1. `config.conf`, `opening.conf`, `economy.conf`, `commands.conf` and every window file the jar ships that is missing
   is written. The example crates, keys, rewards, envoys and animations are written only into a folder that does not
   exist yet.
2. A key the release added is written into your file with the comment that explains it, at its default.
3. A value you set is never changed. A list of names you chose (reroll groups, milestone rungs, luck nodes, claim
   providers, economy placeholders and bridges) stays yours: an entry you removed is not written back.
4. A block an earlier release kept in `config.conf` moves to `opening.conf` or `economy.conf`.
5. Your file as it was goes to `backups/`; the last five copies of each file are kept.

The merge writes the file again from what it holds, so alignment inside a block may move. A key you deleted comes back
at its default, which is why every switch says in its comment what turning it off means.

Related: [config.conf](config-conf.md), [Installation](../getting-started/installation.md)
