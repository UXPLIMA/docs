---
title: Bots
order: 5
description: The three difficulties, whether they call last card, and the avatars they wear.
icon: cpu
---

Empty seats can be filled by bots, so a table is playable with one human.

## The three difficulties

```lua
Bot = {
    Difficulties = {
        Easy   = { randomness = 0.5,  callsLast = false },
        Medium = { randomness = 0.15, callsLast = true },
        Hard   = { randomness = 0,    callsLast = true },
    },
    DefaultDifficulty = "Medium",
},
```

| Field | What it does |
|---|---|
| `randomness` | Chance from 0 to 1 of playing a random legal card instead of the best one |
| `callsLast` | Whether it calls last card in time |

`randomness` is the whole difficulty model, and it works well for this game because the
best play is usually obvious: dump the expensive card, keep the colour you have most of,
save the wild. A bot that does that half the time is a beatable opponent rather than a
weak one.

`Hard` at zero randomness always plays its best card. It is not unbeatable: this is a game
with a shuffled deck and hidden hands, so a good player loses to a bad one regularly.

## callsLast is the difficulty that players notice

An `Easy` bot forgets to call its last card and takes the two-card penalty, which is
exactly the mistake a distracted human makes. It is the single most human thing in the
setting.

Turning it on for `Easy` makes the difficulty gap much narrower, because the endgame is
where games are decided.

## Timing

```lua
Timing = {
    BotMoveDelay = 0.9,
    BotLoadMinSeconds = 0.5,
    BotLoadMaxSeconds = 8,
},
```

| Field | Default | What it does |
|---|---|---|
| `BotMoveDelay` | `0.9` | Pause before a bot plays |
| `BotLoadMinSeconds` | `0.5` | Minimum time the loading splash stays up |
| `BotLoadMaxSeconds` | `8` | Longest wait for a bot avatar before starting anyway |

Nearly a second per bot move is deliberate. With three bots and no delay, a human's turn
comes back before they have seen what happened.

## Avatars

```lua
UseFriendAvatar = true,
FallbackUsernames = { "builderman", "Shedletsky", … },
```

With `UseFriendAvatar = true` a bot borrows the appearance of a random friend of the
player. Failing that it uses one of twelve fallback usernames.

The shipped fallbacks are well-known Roblox accounts. They are recognisable, which reads as
a wink rather than as filler, and none of them belong to you.

<Callout type="tip" title="Consider replacing the fallback usernames">

Twelve famous accounts sitting at your card table is a fine default and a slightly odd look
for a serious game. If your experience has its own characters, put their usernames here
instead.

The list is used in order of availability, so it needs several entries to fill a
four-seat table with distinct opponents.

</Callout>

## Do bot games count

```lua
Leaderstats = {
    CountBotGames = true,
},
```

On by default: wins and losses against bots count. There is no separate switch for points.

That is more generous than the equivalent settings elsewhere in the range, and it is worth
reviewing if you run public boards. A player can sit alone at a table with three `Easy`
bots and win most of them.

Setting `CountBotGames = false` records nothing from bot games, which makes bot play pure
practice.
