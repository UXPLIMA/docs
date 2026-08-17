---
title: uxrDailyRewards
order: 10
description: A daily login ladder with streaks, a skip-the-wait product, and reward code you write yourself.
icon: calendar-check
---

A login reward ladder. A player claims day 1, waits out a cooldown, claims day 2, and
keeps going. Miss a window and the streak goes back to the start. The panel opens by
itself when something is waiting to be claimed.

What each day actually gives is a function you write. The system owns the timing, the
streak, the saving and the panel; it does not decide what a reward is.

## Pages

- [Setup](setup.md)
- [Settings](settings.md)
- [The reward ladder](rewards.md)
- [Streaks and cooldowns](streaks.md)
- [Skip the wait](skip.md)
- [The interface](interface.md)
- [Hooks and API](hooks.md)

## What it does

| | |
|---|---|
| Ladder length | However many entries you write, no upper bound |
| Timing | A rolling cooldown in seconds, not a calendar day |
| Streak | Resets to day 1 after a configurable number of missed windows |
| Looping | Optional restart at day 1 after the last day |
| Monetisation | Optional Developer Product that clears the current cooldown |
| Saving | One `DataStore` profile per player, autosaved |
| Interface | A `ScreenGui` you can re-skin freely, with a documented instance contract |

## What it does not do

It does not give coins, items, pets or gamepasses on its own. Every shipped day has an
empty `Grant` function waiting for your code. That is deliberate: the system has no
opinion about your economy, so it never fights it.

It is also not a calendar. Claiming at 23:00 and again at 00:30 the next day is one
claim, not two, because the cooldown is measured from the last claim. See
[Streaks and cooldowns](streaks.md).
