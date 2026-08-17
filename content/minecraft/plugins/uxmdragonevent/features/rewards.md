---
title: Rewards
order: 36
icon: gift
---

Four reward categories, in `rewards.yml`, in one of two modes.

```yaml
used-rewards: STATIC   # or RANDOMIZED
```

Both modes are configured in the file at once; `used-rewards` picks which section is read. That
makes switching a one-line change, and it means you can prepare a randomized table without turning it
on.

## The categories

| Category | Given to |
|---|---|
| `each-last-hit` | The last player to hit the dragon before each rebirth |
| `final-last-hit` | The last player to hit it before the event ends |
| `event-end.places` | The top three by total damage |
| `event-end.every-other-player` | Everyone else who took part |

The two last-hit rewards are what make the middle of a fight matter. Damage decides the leaderboard,
but the killing blow on each rebirth is its own prize, and a player who cannot win on damage can
still win one of those.

## Static rewards

```yaml
static:
  each-last-hit:
    - 'eco give %player_name% 500'
    - 'msg: <rainbow>Congrats, you were the last to hit dragon before it died and reborn!'

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

Each entry is a console command with `%player_name%` for the recipient. An entry starting with
`msg:` is sent to the player as a MiniMessage message instead of being run as a command.

## Randomized rewards

```yaml
randomized:
  each-last-hit:
    reward_1:
      - 'eco give %player_name% 500'
    reward_2:
      - 'eco give %player_name% 250'
```

Each category holds several `reward_N` keys and one is picked at random. Every list inside the chosen
key runs.

The keys must be named `reward_1`, `reward_2` and so on. There is no weighting — each is equally
likely — so to make one outcome rarer, add more copies of the common one.

Randomized rewards give an event replay value that a fixed table does not: winning first place twice
does not produce the same prize twice.

## Placeholders

`%player_name%` is the recipient. Commands run from console, so anything your other plugins expose
works: `eco give`, `lp user ... parent addtemp`, `crate givekey`, `give`.

<Callout type="warning" title="Rewards are the event's economic output">

Three events a day, each paying `every-other-player` 1000 to everyone who joined, is a large amount
of money entering your economy every day. Set `event-price` against it, and prefer items and keys
over currency for the places — a dragon head is a trophy, and money is inflation.

</Callout>

## Nobody wins

If the dragon is not killed within `dragon-event-time`, the event ends with the `timed-out` message
and **no rewards at all** — not even `every-other-player`. Players who paid to enter get nothing.

That is the correct behaviour for a challenge, but it is worth knowing when you tune the dragon's
health: an event that times out regularly is one players stop paying to enter.
