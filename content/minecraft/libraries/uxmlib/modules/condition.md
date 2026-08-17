---
title: uxmlib-condition
order: 28
icon: git-branch
---

A declarative condition engine and a config-driven action engine. Both parse once and run many
times, so a gate defined in a config file costs a comparison at call time rather than a parse.

## Conditions

```java
ConditionList gate = ConditionList.of(
        PlaceholderCondition.parse("%player_level% >= 30"),
        Text.mini("<red>You need level 30"));

boolean allowed = gate.test(ConditionRequest.forPlayer(player));
```

A condition is an operand comparison with placeholders resolved through an injected
`OperandResolver` — which is what keeps the engine independent of PlaceholderAPI while working with
it.

The failure message travels with the condition. A gate that fails knows what to tell the player, so
the call site does not have to map failures to messages.

## Operators

| Symbol | Meaning |
|---|---|
| `==` | Equal — numeric when both sides are numbers, otherwise exact string equality |
| `!=` | Not equal |
| `>=` | Greater or equal — numeric only; a non-numeric operand is false |
| `<=` | Less or equal — numeric only |
| `>` | Greater than — numeric only |
| `<` | Less than — numeric only |
| `?=` | Contains — the left operand contains the right, always a text test |
| `*` | Glob — `*` matches any run, `?` matches one character, must match in full |
| `\|\|` | Alternation — the right side is a `\|`-separated list, left passes if it equals any branch |

Symbols are matched longest-first, so `>=` is found before `>`, and `*` is tried last.

The numeric operators returning **false** rather than throwing on a non-numeric operand is
deliberate: a placeholder that resolves to an empty string because a plugin is missing should fail
the gate, not break the caller.

## Failure policy

`FailurePolicy` decides what happens when a list has several failing conditions — whether to report
the first, or collect them all. Reporting the first is friendlier for a gate a player is expected to
pass; collecting them all suits a requirements list the player is reading.

## Actions

```java
ActionList.parse(List.of(
        "[message] <green>Hi %player_name%",
        "[console] heal %player_name%")).run(context);
```

Parsed once into closures, then run in order.

| Prefix | Does | Payload |
|---|---|---|
| `[message]` | Sends a MiniMessage template to the target | Required |
| `[broadcast]` | Sends it to every player | Required |
| `[actionbar]` | Sends it to the target's action bar | Required |
| `[title]` | Shows it as a title | Required |
| `[sound]` | Plays an Adventure sound key | Required |
| `[console]` | Dispatches a command from console | Required |
| `[player]` | Dispatches a command as the target | Required |
| `[close]` | Closes the target's open inventory | None |

Prefixes are matched case-insensitively — `[Message]` and `[MESSAGE]` both parse.

## Why parse once

A config file is read at startup and the actions it defines may run thousands of times. Parsing
strings on every run is work repeated for no reason, and it means a malformed line is discovered at
the worst possible moment rather than at load.

`ActionList.parse` fails at load time with a message naming the bad line. Everything after that is a
closure call.

## The pair

Conditions and actions are the natural pair: a menu item shown when a condition passes, running
actions when clicked; a reward gated on a requirement.

[`uxmlib-gui`](gui.md) uses them through `MenuConditions`, so a menu defined in HOCON gets both
without any code beyond registering the named actions it may call.
