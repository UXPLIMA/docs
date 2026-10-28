---
title: Presentation
order: 39
description: Sounds, titles, the second boss bar, messages and the commands run when an event starts and ends.
icon: sparkles
---

How the event announces itself, in `config.yml`.

## Who sees what

| Moment | Sound and title go to | Message |
|---|---|---|
| `start` | Everyone online | `start` to everyone |
| `respawn`, when the dragon is reborn | Players in the event | None |
| `end` | Everyone online | `end` to everyone, at every end |

## Sounds

```yaml
sounds:
  start:
    enable: true
    sound: 'ENTITY_LIGHTNING_BOLT_THUNDER'
    volume: 1.0
    pitch: 1.0
  end: ...
  respawn: ...
```

`sound` takes a Minecraft sound name in capitals, such as `ENTITY_ENDER_DRAGON_GROWL`. An unknown name
is reported in the console and nothing plays.

The start sound reaches the whole server, so pick one that carries without becoming irritating three
times a day.

## Titles

```yaml
titles:
  start:
    enable: true
    title: '<#81e0f7><bold>Dragon Event'
    subtitle: '<green>Starting...'
    fade-in: 1
    stay: 3
    fade-out: 1
    actionbar: ""
```

The times are in seconds. `actionbar` is shown above the hotbar together with the title; leave it
empty to show nothing there.

The `respawn` title takes `%times%`, the kills still needed:

```yaml
respawn:
  title: '<aqua>DRAGON SUDDENLY REBORN!'
  subtitle: '<yellow>You have to kill it <dark_aqua>%times% times <yellow>more!'
```

This is the moment a player who has never seen a rebirth learns how it works, so keep the count in
the subtitle.

## The second boss bar

```yaml
boss-bar:
  enabled: false
  name: "<#00E5FF>Total Rebirths: <#FF5555>%dragonevent_required_kills%♥ <#888888>| <#00E5FF>Remaining: <#FF5555>%dragonevent_rebirths%♥"
  match-progress-to-dragon-health: true
  color: RED
  style: NOTCHED_6
  flags:
    - CREATE_WORLD_FOG
```

A bar of your own, shown to players in the event, beside the vanilla dragon bar. With both on,
players see two bars: set `dragon.boss-bar-visible: false` to keep only this one.

| Key | Values |
|---|---|
| `name` | MiniMessage and placeholders, updated whenever the dragon is hit |
| `match-progress-to-dragon-health` | `true` fills the bar with the dragon's health. `false` keeps it full |
| `color` | `PINK`, `BLUE`, `RED`, `GREEN`, `YELLOW`, `PURPLE`, `WHITE` |
| `style` | `SOLID`, `NOTCHED_6`, `NOTCHED_10`, `NOTCHED_12`, `NOTCHED_20` |
| `flags` | `DARKEN_SCREEN`, `PLAY_BOSS_MUSIC`, `CREATE_WORLD_FOG` |

The notches are only decoration: they do not line up with rebirths or health. `SOLID` is the honest
choice if that bothers you.

The flags are effects on the player's screen: `DARKEN_SCREEN` darkens the sky like a wither fight,
`PLAY_BOSS_MUSIC` plays the dragon music, `CREATE_WORLD_FOG` adds fog. A flag the plugin does not
know is reported once in the console.

## Messages

The `messages` block holds everything the plugin says, in MiniMessage. Every message accepts
placeholders and `%player%`, and a message written as a list is sent as several lines.

A message removed from the file shows its own name, such as `messages.not-active`, and the console
names it once. Plugin updates add missing messages back.

The `end` message is the one players read most closely: it is the leaderboard.

```yaml
end:
  - '<reset> <gold><bold>1st <yellow>%dragonevent_top_name_1% <gray>- <yellow>%dragonevent_top_damage_1%'
  - '<reset> <gray><bold>2nd <white>%dragonevent_top_name_2% <gray>- <white>%dragonevent_top_damage_2%'
  - '<reset> <#CD7F32><bold>3rd <white>%dragonevent_top_name_3% <gray>- <white>%dragonevent_top_damage_3%'
```

It is sent at every end, also when time runs out (followed by `timed-out`) and when staff stop the
event (followed by `force-stop`), so write it so that it reads correctly then too.

The shipped version also lists the prizes in plain text. That list does not read `rewards.yml`: when
you change the rewards, change this message too.

The admin help, `no-arguments-admin`, ends with a link to the old wiki. Point it at these pages or
remove the line.

## Commands on start and end

```yaml
commands:
  start:
    - ""
  end:
    - ""
```

Console commands, written without `/`, run when an event starts and every time one ends. Use them to
trigger a broadcast plugin, a firework show, or a Discord bridge you already run.
