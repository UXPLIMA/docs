---
title: settings
order: 51
icon: sliders-horizontal
---

The `settings` block of `config.yml`, in full.

## Timing and formats

```yaml
settings:
  dragon-event-time: 15
  remaining-time-format: '%days% days %hours% hours %minutes% minutes %seconds% seconds'
  next-event-time-format: 'dd/MM HH:mm'
  ingame-remaining-time-format:
    minutes: "%minutes%m"
    seconds: "%seconds%s"
```

| Key | Meaning |
|---|---|
| `dragon-event-time` | Event length in minutes |
| `remaining-time-format` | Countdown to the next event; `%days%`, `%hours%`, `%minutes%`, `%seconds%` |
| `next-event-time-format` | `SimpleDateFormat` pattern for `%dragonevent_next%` and `_last%` |
| `ingame-remaining-time-format` | The in-event clock, shown as minutes until under one, then seconds |

The in-event clock switches from `minutes` to `seconds` automatically in the last minute — the two
formats are the same value rendered differently, not two different timers.

## Joining

```yaml
settings:
  join-command:
    enabled: false
    require-permission: true
  permission-required: false
  price-required: true
  event-price: 500
  price-required-for-every-join: true
  same-ip-limit: 0
  keep-stats-on-leave: true
  auto-join-after-leaving: true
```

Documented on [Joining](../features/joining.md).

## Worlds

```yaml
settings:
  lobby-world-name: world
  event-world-name: 'dragonevent_end'
  worldguard-region-name: dragonportal
  spawn-type: PLATFORM
  delete-end-crystals: true
  end-portals:
    all-end-portals-leads-to-event: false
    include-all-worlds: false
    list-type: WHITELIST
    world-list:
      - "world_the_end"
    accepted-portal-types:
      - END_PORTAL
```

Documented on [Worlds](../features/worlds.md) and
[Setting up the event](../getting-started/setup.md#4-the-portal).

## Flight

```yaml
settings:
  auto-flight: false
```

Gives flight on entry to players holding `dragonevent.fly`. See
[Joining](../features/joining.md#flight).

## Command blocking

```yaml
command-blocking:
  disable-all-commands: true
  list-type: WHITELIST
  list:
    - "/example"
```

Not inside `settings`, but closely related.

`/dragon` subcommands are always allowed. Players with `dragonevent.usecmds` bypass everything here.

| `list-type` | The list contains |
|---|---|
| `WHITELIST` | Commands players **may** use |
| `BLACKLIST` | Commands players **may not** use |

Use a whitelist. A blacklist has to enumerate every teleport, home, warp and shop command on the
server, including the ones another plugin adds next month — and each one you miss is a way for a
player to leave the arena while still holding an event session.

`disable-all-commands: false` means the lists are not consulted at all.

## Top of the file

```yaml
config-version: "..."
license-key: ""
```

`config-version` is written by the plugin and used for migration between versions — leave it.
`license-key` comes from your UXPLIMA panel.
