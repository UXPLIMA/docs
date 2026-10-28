---
title: settings
order: 51
description: The settings block of config.yml in full, with a pointer to the page for each part.
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
| `remaining-time-format` | `%dragonevent_remaining%`, the time until the next scheduled event |
| `next-event-time-format` | `%dragonevent_next%` and `%dragonevent_last%`. A Java date pattern: `dd/MM HH:mm` gives `28/10 20:00` |
| `ingame-remaining-time-format` | `%dragonevent_remaining_game%`, the time left in the running event |

The time left in the event is shown in whole minutes until less than a minute remains; then it
switches to seconds.

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
  keep-stats-on-leave: true
  auto-join-after-leaving: true
  same-ip-limit: 0
  auto-flight: false
```

See [Joining](../features/joining.md).

## The entrance

```yaml
settings:
  lobby-world-name: world
  worldguard-region-name: dragonportal
  end-portals:
    all-end-portals-leads-to-event: false
    include-all-worlds: false
    list-type: WHITELIST
    world-list:
      - "world_the_end"
    portal-behavior-when-event-is-inactive: REGION_CHECK
    accepted-portal-types:
      - END_PORTAL
```

See [Setting up the event](../getting-started/setup.md#3-the-entrance).

## The event world

```yaml
settings:
  event-world-name: 'dragonevent_end'
  spawn-type: PLATFORM
  delete-end-crystals: true
```

See [Worlds and the border](../features/worlds.md).

## Top of the file

```yaml
config-version: "2.6.3"
license-key: ""
```

`config-version` is written by the plugin and tells it which settings to add on an update: leave it
alone. `license-key` is the key from your UXPLIMA purchase.

## Debug output

When something does not work and the console says nothing, add these two lines to the top of
`config.yml` and run `/dragon reload`:

```yaml
debug: true
debug-to-ops: false
```

`debug` writes what the plugin is doing to the console, including every Discord error.
`debug-to-ops: true` sends the same lines to operators in game. Remove both when you are done; the
next plugin update removes them anyway.
