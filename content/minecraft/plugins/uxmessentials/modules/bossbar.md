---
title: Boss Bars
order: 9265
description: Bars at the top of the screen that stay up, run for an event's dates, or are announced for a while.
icon: panel-top
---

The bossbar module draws the bars you write at the top of every player's screen. A bar can stay up, come and go
with an event's start and end, or show only while a command announces it. Every bar whose turn it is shows at
once, stacked in the order you write them, and each player reads them in their own language.

Module `bossbar` · disabled by default · `modules/bossbar/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/bossbars` | Hide or show your boss bars, or announce one. | `uxmessentials.bossbar.use` |
{/* /generated */}

| Usage | What it does |
|---|---|
| `/bossbars` | Hide or show your own bars. |
| `/bossbars toggle\|on\|off` | The same, said outright. |
| `/bossbars toggle\|on\|off <player>` | Hide or show another player's bars. Needs `uxmessentials.bossbar.others`. |
| `/bossbars announce <bar> <time>` | Put a bar up for everybody for that long, such as `30s`, `5m` or `1h30m`. |
| `/bossbars send <player> <bar> <time>` | Put a bar up for one player for that long. |

The command is `/bossbars`, not `/bossbar`, because the vanilla `/bossbar` is what map makers and data packs
run. Rename it in `commands.conf` like any other command.

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.bossbar.announce` | op | /bossbars announce \<bar> \<time> for everybody and /bossbars send \<player> \<bar> \<time> for one player. |
| `uxmessentials.bossbar.others` | op | /bossbars toggle\|on\|off \<player> to hide or show another player's boss bars. |
| `uxmessentials.bossbar.use` | everyone | /bossbars, /bossbars toggle\|on\|off to hide or show your own boss bars. |
| `uxmessentials.module.bossbar` | op | Hot-reload / inspect the bossbar module (bars that stay up, timed bars and announced bars). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `refresh-ticks` | `20` | How often every player's bars are drawn again, in server ticks (20 ticks = one second). A bar that counts down or reads a placeholder changes at this pace. |
| `hidden-by-default` | `false` | Whether a player who never ran /bossbars sees no bars until they turn them on. |
| `time-zone` | `""` | The time zone a start or an end below is read in. Empty is the server's own. Examples: "Europe/Istanbul", "UTC". |
| `bars` | `[...]` | The bars, top first. Every bar whose turn it is shows at once. |
{/* /generated */}

## Writing a bar

```hocon
bars = [
  {
    id = "website"
    text = "@custom.bar.website"
    color = "GREEN"
    style = "NOTCHED_20"
    progress = "100"
  }
  {
    id = "double-xp"
    text = "<white>ᴅᴏᴜʙʟᴇ xᴘ</white> <green>ᴀᴄᴛɪᴠᴇ</green> <gray>({remaining})</gray>"
    color = "GREEN"
    style = "NOTCHED_20"
    start = "2026-11-01 18:00"
    end = "2026-11-08 18:00"
  }
]
```

- **The text** is MiniMessage, or a catalogue line written as `@key` that each player reads in their own
  language. Built-in `{tokens}`, PlaceholderAPI placeholders and `%anim_<name>%` animations work in the text,
  the colour, the style and the progress, as they do on the scoreboard.
- **A timed bar** is up from `start` until `end` and gone after it. `{remaining}` writes the time left in the
  reader's words, the largest unit first: `5d 8h 52m` in English, `5g 8s 52dk` in Turkish. The bar drains from
  full at the start to empty at the end unless you give it a `progress` of your own. Dates are read in
  `time-zone`, or written with their own offset.
- **An announcement bar** (`announcement = true`) shows only while `/bossbars announce` or `/bossbars send` puts
  it up. Any bar can be announced: a bar that is up already simply counts the announcement's time instead.
- **A condition** decides who sees a bar, in the scoreboard's grammar: `permission:<node>`, `world:<name>`,
  `gamemode:<mode>`, a comparison such as `%server_online% >= 10`, negated with `!` and combined with `&&` or
  `||`.

## Notes

- **A player's choice is kept.** `/bossbars` stores it with the player, so it survives a relog.
  `hidden-by-default = true` hides the bars from everybody who has not turned them on.
- **An announcement lives in memory.** A restart ends it; a timed bar does not need one, because its dates are in
  the file.
- **`/uxmess reload` reads the file again** and draws every player's bars at once.
- **Bedrock players see the bars too.** Geyser draws a boss bar natively.
