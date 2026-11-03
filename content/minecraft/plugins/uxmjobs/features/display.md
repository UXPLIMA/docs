---
title: Display
order: 314
description: The action bar, the progress bar, the chat prefix, dialogs, the level up, payment actions and milestones.
icon: monitor
---

`display.conf`:

| Block | Key | Default | Meaning |
|---|---|---|---|
| `action-bar` | `enabled`, `default-on`, `linger-seconds` | true, true, 3 | The line that says what a payment was worth; `/jobs toggle` |
| `boss-bar` | `enabled`, `default-on`, `linger-seconds` | true, false, 5 | Progress to the next level; `/jobs bar` |
| `chat-prefix` | `enabled` | false | Formats chat with `chat.format` and the jobs a player works |
| `dialogs` | `enabled` | true | Native screens to confirm a join fee, a leave and a contract |
| `level-up` | `sound`, `firework`, `firework-colour`, `firework-shape`, `broadcast-from-level`, `actions` | see below | The celebration |
| `paid` | `actions`, `empty-account-notice-seconds` | `[]`, 60 | What runs on every payment |

## Chat prefix

Off by default: it formats the whole line, so on a server that runs a chat plugin whichever is asked last wins. A
player who works no job is never touched.

## Dialogs

The screens the client draws itself, from Minecraft 1.21.6: joining a job with a fee, leaving a job, signing a contract,
and naming a zone. Off, or on an older server, each falls back to the chest window (a Floodgate form on Bedrock) or a
line of chat.

## Level up

`sound` (`entity.player.levelup`), a firework (`firework`, `firework-colour` `#4ecca3`, `firework-shape` one of `ball`,
`ball_large`, `star`, `burst`, `creeper`), and a broadcast from `broadcast-from-level` (25; 0 for none). `actions` adds
anything else, each line with `{delay=N, repeat=N, chance=0.5, if=...}`; `<player>`, `<job_name>` and `<level>` are
filled in.

## Payment actions

`paid.actions` runs on every payment, so it ships empty. `<player>`, `<money>`, `<experience>` and `<points>` are
filled in.

## Milestones

`progress-alerts` in `config.conf` runs actions at percentages of the way through a level:

```hocon
progress-alerts {
  at {
    50 { actions = ["[actionbar] <subtext><job_name>: <accent>halfway"] }
    90 { actions = ["[actionbar] <subtext><job_name>: <accent>nearly there", "[sound] minecraft:block.note_block.pling 0.5 1.8"] }
  }
}
```

Crossed, not reached: a payment from a tenth to nine tenths fires everything between. A level up starts the climb
quiet. `<player>`, `<job_name>` and `<percent>` are filled in. It ships empty.

Related: [display.conf](../config/display-conf.md)
