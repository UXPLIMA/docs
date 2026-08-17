---
title: Presentation
order: 39
icon: sparkles
---

Sounds, titles, the extra boss bar and the messages the event announces itself with.

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

Three moments: the event starting, the event ending, and the dragon being reborn. Any Bukkit sound
name.

The start sound plays server-wide — it is the announcement — so pick something that carries without
being irritating three times a day.

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

Same three moments. Times are in seconds. `actionbar` is shown instead of nothing if you set it;
leave it empty to skip.

The `respawn` title takes `%times%` — how many kills remain:

```yaml
respawn:
  title: '<aqua>DRAGON SUDDENLY REBORN!'
  subtitle: '<yellow>You have to kill it <dark_aqua>%times% times <yellow>more!'
```

This is the moment the rebirth mechanic is explained to a player who has never seen it. Keep the
count in the subtitle.

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

This is a second bar, in addition to the vanilla dragon bar. With both on, players see two — set
`dragon.boss-bar-visible: false` if you want only this one.

| Key | Values |
|---|---|
| `color` | `PINK`, `BLUE`, `RED`, `GREEN`, `YELLOW`, `PURPLE`, `WHITE` |
| `style` | `SOLID`, `NOTCHED_6`, `NOTCHED_10`, `NOTCHED_12`, `NOTCHED_20` |
| `flags` | `DARKEN_SCREEN`, `PLAY_BOSS_MUSIC`, `CREATE_WORLD_FOG` |
| `match-progress-to-dragon-health` | The bar fills with the dragon's health rather than the rebirth count |

`NOTCHED_6` with `required-kills: 2` does not line up. If you want the notches to mean something,
match the style to your kill count, or leave it `SOLID`.

The three flags are client effects: `DARKEN_SCREEN` gives the wither's dark sky, `PLAY_BOSS_MUSIC`
plays the dragon theme, and `CREATE_WORLD_FOG` fogs the world. All three at once is a lot; the
default picks one.

## Messages

The `messages` block in `config.yml` holds everything the plugin says, in MiniMessage.

The `end` message is the one worth writing carefully — it is the leaderboard everyone reads:

```yaml
end:
  - '<reset> <gold><bold>1st <yellow>%dragonevent_top_name_1% <gray>- <yellow>%dragonevent_top_damage_1%'
  - '<reset> <gray><bold>2nd <white>%dragonevent_top_name_2% <gray>- <white>%dragonevent_top_damage_2%'
  - '<reset> <#CD7F32><bold>3rd <white>%dragonevent_top_name_3% <gray>- <white>%dragonevent_top_damage_3%'
```

The shipped version also lists the prizes in plain text. That list is written by hand — it does not
read `rewards.yml` — so if you change the rewards, change this message too, or it will advertise
prizes nobody receives.

## Commands on start and end

```yaml
commands:
  start:
    - ""
  end:
    - ""
```

Console commands run when the event starts and ends. This is where you trigger a broadcast plugin, a
firework show, or an announcement in a Discord bridge you already run.
