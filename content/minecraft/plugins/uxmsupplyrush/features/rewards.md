---
title: Rewards
order: 34
description: Fixed, random and chance-based rewards for each rank, and how winners get them.
icon: gift
---

Rewards are written in the `rewards` section of each [event file](../config/event-files.md). Each
rank you list has its own reward, and `participation` covers everyone else who deposited anything.
Rewards are commands and messages, so anything your other plugins can give is a possible prize.

## The three reward types

```yaml
rewards:
  mode: "STATIC"
  roll: "PLACE"
```

| `mode` | Each rank gets |
|---|---|
| `STATIC` | The same reward every time |
| `RANDOMIZED` | One of several rewards, picked at random with equal chances |
| `DYNAMIC` | Rewards with their own chance, in percent |

`events/example-rush.yml` contains a full example of all three: only the section that matches
`mode` is used.

### STATIC

```yaml
static:
  places:
    1:
      display: "<#FFD166>10000 coins"
      actions:
        - "[console] eco give %player_name% 10000"
        - "[player_message] <#62D97E>You finished 1st!"
  participation:
    display: "<#FFD166>500 coins"
    actions:
      - "[console] eco give %player_name% 500"
```

### RANDOMIZED

Give a rank as many lists as you like, under any names. One of them is picked, each as likely as the
others.

```yaml
randomized:
  places:
    2:
      display: "<#FFD166>5000 coins <#A8A8A8>or <#7BDFF2>10x Diamond"
      actions-a:
        - "[console] eco give %player_name% 5000"
      actions-b:
        - "[console] give %player_name% diamond 10"
```

### DYNAMIC

Each entry has a `chance` out of 100. The entries are tried from top to bottom and the first one
that succeeds is given, so put the rarest prize first. With `give-at-least-one-reward: true`, a
player whose every roll failed still gets one of the entries, picked at random.

```yaml
dynamic:
  places:
    1:
      display: "<#FFD166>Up to 20000 coins"
      give-at-least-one-reward: true
      grand-prize:
        chance: 10.0
        actions:
          - "[console] eco give %player_name% 20000"
      regular-prize:
        chance: 80.0
        actions:
          - "[console] eco give %player_name% 8000"
```

## Shared or separate rolls

`roll` only matters for `RANDOMIZED` and `DYNAMIC`:

| `roll` | Result |
|---|---|
| `PLACE` | One roll per rank: tied players, and everyone on participation, get the same result |
| `PLAYER` | Every player rolls for themselves |

## Actions

| Action | What it does |
|---|---|
| `[console] <command>` | Runs a command from the console |
| `[player] <command>` | Runs a command as the winner |
| `[player_message] <text>` | Sends the winner a message |
| `[sound] <sound>` | Plays a sound to the winner |

`%player_name%` is replaced with the winner's name, and with PlaceholderAPI installed every other
placeholder works too. `[wait]` lines are skipped in rewards, and the console warns about them when
the file is loaded.

## Showing the rewards

`display` is the text players see for a reward. `<reward:1>`, `<reward:2>` and so on put the display
of that rank into the event's announcements and into `rewards-explanation`, which is what the
**Rewards** button in the event menu shows.

## How winners are paid

Rewards are paid the moment a run ends. A winner who is offline gets theirs the next time they
join, on any server of the network. Every reward is paid exactly once, and a run ended with
**Stop Unpaid** pays nothing.

Related: [Ranking](ranking.md), [Event files](../config/event-files.md)
