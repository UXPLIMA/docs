---
title: Your first claim
order: 204
description: Claim day one, read the window, give yourself another day, and change what a day gives.
icon: play
---

This walk-through uses a fresh install and an operator account.

## 1. The reminder

Join. Five seconds later a title reads `Daily reward` with `Day 1 is waiting for you.` under it, the action bar says
the same, and a pling sounds.

## 2. Claim

Run `/dailyrewards`. The window shows 28 days in four rows. Day 1 is a glowing chest minecart: hover it to read
what it gives at your rank, such as `Reward 8 Bread, 50 experience`, and `State Ready`. Click it:

```
You claimed day 1. Your streak is 1.
```

The bread arrives, the experience bar fills a little, the level up sound plays and totem particles burst: that last
part is the celebration, run after every claim. Day 1 turns into a plain minecart, and day 2 into a command block
minecart: next up. The clock at the bottom shows the time to the next claim.

An operator holds every rank node by default, so on a server without LuckPerms the claim pays at the best rank on the
ladder, `mvp`: 24 bread and 150 experience. Test with a player account to see the default reward.

## 3. Try again

```
/dailyrewards claim
You have claimed today. Come back in 23h 59m 41s.
```

## 4. Give yourself a day

```
/uxmdailyrewards grant Steve 7
Day 7 is given to Steve.
```

Steve gets day 7's reward at his rank, and his streak and next day do not move. Use it to make up a day a player
lost to a crash.

## 5. Change a day

Open `plugins/uxmDailyRewards/days/2.conf` and add a line:

```hocon
reward = [
  "[console] give %player% iron_ingot 4",
  "[give-money] exp 60",
  "[message] <good>Day %day%, streak %streak%. Keep going!"
]
```

Run `/uxmdailyrewards reload`. The next claim of day 2 sends the message too.

## 6. Start over

```
/uxmdailyrewards reset Steve
The progress of Steve is cleared.
```

## Next

- [Day files](../rewards/day-files.md): every key of a day.
- [Ranks](../rewards/ranks.md): a better reward for a group.
- [The calendar](../rewards/calendar.md): the length, the window and the grace.
