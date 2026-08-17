---
title: uxrHandcuff
order: 32
description: Cuff, grab, search, take down and jail, with bail, admin commands and an arrest log.
icon: handcuffs
---

A police roleplay toolkit. An officer holding the handcuffs can cuff a suspect, walk them
somewhere, search what they were carrying, take a weapon off them and put them in a cell for
a typed sentence with typed charges.

The suspect gets a countdown, an arresting officer, a charge sheet and, if you enable it, a
way to buy their way out.

## Pages

- [Setup](setup.md)
- [Teams and permissions](teams.md)
- [Actions](actions.md)
- [Arrest and jail](jail.md)
- [Bail](bail.md)
- [The golden handcuff](golden.md)
- [Admins and chat commands](admin.md)
- [The arrest log](webhook.md)
- [Settings](settings.md)

## What it does

| | |
|---|---|
| Restraint | A three-state machine: free, cuffed, grabbed |
| Confiscation | Cuffing takes every tool; uncuffing gives them back |
| Search | List what a grabbed suspect was carrying, take it or destroy it |
| Takedown | Force a cuffed suspect to the ground |
| Rights | A speech bubble everybody sees, on a cooldown |
| Arrest | A form: sentence length, charges, a typed reason |
| Jail | Team swap, respawn, a live countdown, saved across sessions |
| Global jail | A separate, usually shorter, sentence type with its own limits |
| Bail | In-game currency or a Robux developer product |
| Leaving | Logging off while cuffed jails you on return |
| Admins | Six chat commands that bypass the team rules |
| Logging | Every arrest posted to a webhook |
| Free for all | One switch that removes teams entirely |

## The shape of it

Everything an officer may do is decided by an **officer team plus target team** pair in
`Config/Teams.luau`. If the pair is not listed, the officer can do nothing to that target.

That whitelist is also the security model. The server checks it on every action, along with
distance, restraint state, whether the officer is holding the tool, and a rate limit.

If your game is not police versus civilians, `Config/FreeForAll.luau` replaces the whole
matrix with one shared entry.
