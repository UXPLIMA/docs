---
title: Rewards
order: 36
description: Who gets rewarded, and the three ways to write rewards.yml, as fixed rewards, random picks or chance rolls.
icon: gift
---

Rewards live in `rewards.yml`. The file holds three sections, one per mode, and `used-rewards` picks
which one is read:

```yaml
used-rewards: STATIC   # or RANDOMIZED, or DYNAMIC
```

The other two sections are ignored, so you can prepare a new table without turning it on. A value
that is not one of the three falls back to `STATIC` with a console warning.

## Who gets what

| Category | Given to |
|---|---|
| `each-last-hit` | The player who lands the killing blow on each death but the last, right away |
| `final-last-hit` | The player who lands the killing blow on the last death |
| `event-end.places.<place>` | The players ranked by damage at the end: `1`, `2`, `3` and as many places as you write |
| `event-end.every-other-player` | Everyone else still in the event at the end |

The killing-blow rewards only go to a player in the event; if the dragon dies some other way, nobody
gets them. A player can receive more than one category: the final killing blow and first place, for
example.

The end-of-event rewards go to players in the event when the dragon dies for the last time, after
they are back at the lobby spawn. A player who left the event, or went offline, gets nothing.

Rewards are only given when the dragon is killed. An event that runs out of time, is stopped with
`/dragon stop`, or ends with a reload or a server stop, gives no end-of-event rewards. The
`each-last-hit` rewards already given during it are kept.

## Writing a reward

Each reward is a list of lines:

```yaml
- 'eco give %player_name% 500'
- 'msg: <rainbow>Congrats, you were the last to hit the dragon!'
```

| Line | What happens |
|---|---|
| A command | Run from the console. Write it without the `/` |
| `msg: ` and a message | Sent to the player, in MiniMessage. The space after `msg:` is required |

Both accept any PlaceholderAPI placeholder for the player receiving the reward. `%player_name%` is
their name.

## STATIC: a fixed reward

```yaml
static:
  each-last-hit:
    - 'eco give %player_name% 500'
  final-last-hit:
    - 'eco give %player_name% 500'
  event-end:
    places:
      1:
        - 'give %player_name% dragon_egg 1'
        - 'give %player_name% dragon_head 3'
      2:
        - 'give %player_name% dragon_head 2'
      3:
        - 'give %player_name% dragon_head 1'
    every-other-player:
      - 'eco give %player_name% 1000'
```

Every category is one list, given in full every time.

## RANDOMIZED: one reward picked at random

```yaml
randomized:
  each-last-hit:
    reward_1:
      - 'eco give %player_name% 500'
    reward_2:
      - 'eco give %player_name% 250'
```

Each category holds several rewards under names of your choice, and one of them is picked, each with
the same chance. To make one outcome more likely, add it more than once under different names.

The same applies to every place: `places.1.reward_1`, `places.1.reward_2`, and so on.

## DYNAMIC: rewards rolled by chance

```yaml
dynamic:
  each-last-hit:
    give-at-least-one-reward: true
    reward_1:
      chance: 70.0
      commands:
        - 'eco give %player_name% 500'
    reward_2:
      chance: 30.0
      commands:
        - 'eco give %player_name% 250'
```

| Key | Meaning |
|---|---|
| `chance` | From `0.0` to `100.0`. A reward without it has a 1% chance |
| `commands` | The reward's lines, written as above |
| `give-at-least-one-reward` | Per category. When every roll fails, one reward of the category is given at random instead of nothing. Off when not written |

The rewards are rolled one after another, in the order they are written, and the first one that
succeeds is given. The rest are not rolled, so a player gets at most one reward per category.

That means the chances do not add up like shares. In the example above:

| Outcome | Chance |
|---|---|
| `reward_1` | 70% |
| `reward_2` | 30% of the remaining 30%, so 9% |
| Nothing | 21%, which `give-at-least-one-reward: true` splits between the two rewards at random |

Write the rarest reward first and a common one last. To make sure something is always given, give the
last reward `chance: 100.0`.

<Callout type="warning" title="Rewards are money entering the economy">

Three events a day, each paying `every-other-player` 1000 to everyone who joined, puts a lot of money
into the economy every day. Set `event-price` against it, and prefer items and keys over money for the
places: a dragon head is a trophy, money is inflation.

</Callout>

## The end message

The default `end` message in `config.yml` lists the prizes in plain text. It does not read
`rewards.yml`: when you change the rewards, change the message too, or it will advertise prizes nobody
receives. See [Presentation](presentation.md#messages).
