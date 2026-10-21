---
title: Running commands
order: 3
description: The three entry points, the syntax, modifiers, chaining and argument types.
icon: terminal
---

## Three ways in

| Route | Opened with |
|---|---|
| The chat prefix | Type `u!kill me` in the chat window |
| The command bar | Press `;` |
| The panel | Press `/`, then browse the Commands page |

All three end up in the same place: one server-side dispatcher that parses the text,
checks the rank, resolves the targets and runs the code.

The prefix is configurable:

```lua
Prefix = "u!",
```

It is cosmetic. The server strips it before looking a command up, so the internal names
stay bare.

<Callout type="info" title="Chat commands are real TextChatCommand objects">

At startup the client registers a `TextChatCommand` for every command and every alias, both
in their written case and in lower case.

That means Roblox's own chat autocomplete lists them, and it also means a command name that
collides with another system's chat command will be one of the two that wins.

</Callout>

## Syntax

```
u!walkspeed nearest 100
u!ban Player1 7d Exploiting in the arena
u!kill "Some Player" 
```

| Rule | |
|---|---|
| Arguments are whitespace separated | Tabs count as spaces |
| Quotes group words | `"Some Player"` is one argument |
| Backslash escapes | `\"` inside quotes gives a literal quote |
| Case | Command names are matched case-insensitively |

The last argument of many commands is marked to absorb the rest of the line, so a ban
reason does not need quoting.

## Modifiers

Modifiers go **before** the command name.

```
u!silent kill all
u!n5 explosion me
u!force ban Player1
```

| Modifier | Does |
|---|---|
| `silent` | No confirmation toast, and no error toast either |
| `force` | Skips the rate limit |
| `n<number>` | Runs the command that many times, capped at 100 |

`force` is only honoured for Admin and above; from anybody lower it is dropped and the rate
limit applies as normal.

<Callout type="warning" title="n100 means what it says">

`u!n100 explosion all` detonates a hundred explosions on every player in the server. The
repeat count multiplies by the target count, so on a full server that is thousands of
operations in one statement.

The rate limit does not help here: it gates statements, not repeats.

</Callout>

## Chaining

`&&` separates statements, and each one runs in order.

```
u!ff me && u!god me && u!fly me 100
```

A statement that fails reports its own error and the rest still run. Each statement is
parsed, permission-checked, rate-limited and logged on its own.

## Rate limiting

```lua
CommandDebounce = 0.25,
CommandFrequencies = {
    shutdown  = 5,
    nuke      = 3,
    explosion = 1,
    vote      = 10,
    permrank  = 2,
    rank      = 1,
},
```

`CommandDebounce` is the gap between two runs of the same command by the same player.
`CommandFrequencies` overrides it per command.

A throttled command is dropped silently: no error, no log, nothing happens. That is
deliberate, because the alternative is a stream of "too fast" toasts while somebody holds a
key down.

Set `CommandDebounce = 0` to disable the global gate and keep the per-command overrides.

## Argument types

Each argument in `Commands.luau` declares a type, and the dispatcher parses it before the
command runs.

| Type | Accepts | Notes |
|---|---|---|
| `Players` | A target expression | See [Choosing targets](targets.md) |
| `Number` | A number | Clamped to `min` and `max` rather than refused |
| `String` | Text | With `joinRest`, absorbs the rest of the line |
| `Duration` | `10s`, `5m`, `1h30m`, `7d`, `1y` | Or a bare number of seconds |
| `AssetId` | A positive integer | |
| `Color` | A hex code or a colour name | |
| `Bool` | true or false | |
| `Rank` | A rank name | Validated against the ladder |
| `Team` | A team name | |
| `Material` | A material name | |

Each argument may carry a `default`, so `u!walkspeed` with no target means you, and may be
`optional`, so leaving it out is not an error.

<Callout type="info" title="Numbers are clamped, not rejected">

`walkspeed` declares `min = 1, max = 500`. Asking for 9999 gives 500 rather than an error.

That is the right behaviour for a live moderation tool: the command does something sensible
instead of making the admin retype it, and the limits are still enforced.

</Callout>

### Durations

```
u!tempban Player1 1h30m Cooling off
u!tempmute Player1 10m
```

The units are `y`, `M`, `d`, `h`, `m`, `s`, and they add up, so `1h30m` is 5400 seconds.
A bare number is read as seconds.

Note the case: `M` is a month and `m` is a minute.

## Confirmation

Thirty-four commands are marked as destructive and open a confirmation dialog in the panel
before they run: the bans, the kicks, the shutdowns, the lockdowns, the map and terrain
operations, `crash`, `nuke` and `resetstats` among them.

<Callout type="warning" title="Confirmation is a panel feature only">

Typing the same command in chat runs it immediately. The dialog belongs to the command bar
and the panel, not to the dispatcher.

Treat the chat prefix as the expert route.

</Callout>

## What happens after a command runs

| Step | |
|---|---|
| Analytics | The command and the admin are counted |
| The command runs | Once per resolved target, inside a `pcall` |
| Logging | If the command's `Log` flag is set |
| Webhook | If its `Webhook` flag is set |
| Feedback | A success toast naming the command and who it hit |

A command that errors logs a warning to Output and the rest of the targets still get
processed. One bad target does not abort a server-wide command.

## The description in the log

The log entry is built from the typed arguments, skipping anything left at its default. So
`u!walkspeed Player1 100` logs the target and `speed: 100`, and `u!walkspeed Player1` logs
just the target.

Colours are written as hex, positions as coordinates, and a multi-target argument as a
count. See [Logs, analytics and webhooks](logs.md).
