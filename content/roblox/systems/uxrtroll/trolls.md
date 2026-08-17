---
title: The seven trolls
order: 2
description: What each one does, what Amount and Duration mean, and the chat lines.
icon: flame
---

## The row

```lua
Kill = {
    Enabled = true,
    OneProductId = 3707848824,
    AllProductId = 3707849001,
    Duration = 0,
    Amount = 0,
    Message = "",
    OneMessage = "%s killed %s",
    AllMessage = "%s killed everyone",
},
```

| Field | Meaning |
|---|---|
| `Enabled` | `false` hides both buttons |
| `OneProductId` | The single-target product. `0` hides that button |
| `AllProductId` | The everyone product. `0` hides that button |
| `Duration` | How long a timed troll lasts, in seconds |
| `Amount` | The troll's strength. Means something different per troll |
| `Message` | Only Kick uses it, as the kick reason |
| `OneMessage` | The chat line. First `%s` is the buyer, second is the victim |
| `AllMessage` | The chat line. One `%s`, the buyer |

## The seven

| Troll | Kind | `Duration` | `Amount` |
|---|---|---|---|
| `Kill` | Instant | Ignored | Ignored |
| `Slow` | Timed | 15 | 0.35, a walk speed multiplier |
| `Freeze` | Timed | 8 | Ignored |
| `Fire` | Timed | 8 | 15 damage per second |
| `Fling` | Instant | Ignored | 180, upward force |
| `Explode` | Instant | Ignored | 100 blast damage |
| `Kick` | Instant | Ignored | Ignored |

### Kill

Sets the victim's health to zero. Nothing to tune.

### Slow

Multiplies the victim's current walk speed by `Amount` for `Duration` seconds, then puts it
back.

It multiplies rather than sets, so a game with a non-standard walk speed is handled
correctly, and a player who is already slowed by something of yours ends up slower still.

`0.35` is a noticeable trudge. `0.7` is an annoyance. `0` is a freeze by another name.

### Freeze

Sets walk speed, jump power and jump height to zero, attaches the freeze effect, and
restores all three afterwards.

The three original values are captured at the moment of freezing and restored exactly, so a
game with custom jump settings is not quietly reset to Roblox defaults.

### Fire

Attaches the fire effect and deals `Amount` damage every second for `Duration` seconds.

```lua
Duration = 8,
Amount = 15,
```

That is 120 damage in total, which kills a default hundred-health character with time to
spare. Lower `Amount` to 10 for a survivable burn, or shorten `Duration`.

The damage loop stops when the victim dies, so a corpse does not keep taking hits.

### Fling

Throws the victim straight up at `Amount` force with a matching spin, holds them in a
ragdoll for a moment, then hands control back.

`180` is a good arc. `400` is a launch. Very high values on a small map put players through
the ceiling.

<Callout type="info" title="Fling's Amount also drives Explode">

Explode launches its victim using Fling's `Amount`, not its own. Raising Fling throws
explosion victims further too.

That is worth remembering when you tune Fling for feel and wonder why explosions changed.

</Callout>

### Explode

Emits the explosion effect at the victim, then deals `Amount` blast damage.

| Outcome | What happens |
|---|---|
| They survive | They are flung, using Fling's `Amount` |
| They die | Their body parts scatter |

`Amount = 100` kills a default character outright, so the scatter is what players will
normally see. Lower it to 40 and the flinging becomes the main effect.

### Kick

Removes the player from the game with `Message` as the reason:

```lua
Message = "You were trolled.",
```

That text is the only thing the kicked player sees, so make it clear enough that they do not
think they were banned. Something like `You were trolled, come back` reads better than a
bare full stop.

<Callout type="warning" title="Kick everyone empties your server">

The All button on Kick removes every non-immune player at once, including the people who
were happily playing.

It is the single most disruptive product in the system. Consider leaving `AllProductId` at
`0` for Kick, or pricing it high enough that it is rare.

</Callout>

## Restarting a timed troll

Applying a timed troll to somebody who already has it cancels the first and starts fresh,
so the duration restarts rather than stacking. The original values are restored from the
first application, not from the slowed or frozen state.

A player who leaves has every active effect cleaned up.

## Chat lines

```lua
OneMessage = "%s killed %s",
AllMessage = "%s killed everyone",
```

Display names are substituted, not usernames. Set either to `""` to keep that one troll
quiet while the rest still announce.

A malformed format string is caught and the line is skipped rather than erroring, so a
mistake here costs you an announcement rather than a troll.

## Announcements

```lua
Announce = {
    Enabled = true,
    Color = Color3.fromRGB(255, 170, 0),
    OnCooldown = "Wait %d seconds before using another All troll",
    TargetGone = "They left, so your troll is saved for the next target",
},
```

| Field | Seen by |
|---|---|
| `Enabled` | Master switch for every line, public and private |
| `Color` | The colour of the line in chat |
| `OnCooldown` | Only the player who pressed too soon |
| `TargetGone` | Only the buyer whose target left |

`Enabled = false` silences everything. The trolls still happen; nobody is told about them.

<Callout type="tip" title="Silence is a design choice">

Announcing every troll makes the system feel alive and encourages more purchases. Silencing
it makes trolls feel like acts of nature and keeps your chat readable.

Both work. What does not work is announcing only some trolls, because players notice the
gap and assume the others failed.

</Callout>

## The effect assets

Three parts in `Assets/Shared`, cloned onto the victim:

| Name | Used by |
|---|---|
| `FreezeEffect` | Freeze |
| `FireEffect` | Fire |
| `ExplodeEffect` | Explode |

Timed effects are welded to the victim's root part and cleaned up when the troll ends. The
explosion effect emits its particles once and removes itself after three seconds.

Replace them with your own parts under the same names. A missing asset means no visual and
the troll still lands.
