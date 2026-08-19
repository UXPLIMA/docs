---
title: uxmlib-condition
order: 28
description: A config-driven condition engine, with operators, failure policies and placeholder operands.
icon: git-fork
---

An operator writes `%player_level% >= 30` in a config file and your code enforces it. That is the
whole idea. Its natural pair, the [action engine](actions.md), is what runs when the answer is yes or
no.

Depends on `uxmlib-common` only. It knows nothing about PlaceholderAPI: operand resolution is a seam
you point wherever you like.

## A gate

```java
ConditionList gate = ConditionList.of(
        PlaceholderCondition.parse("%player_level% >= 30"),
        Text.mini("<red>You need level 30"));

boolean allowed = gate.test(ConditionRequest.forPlayer(player));
```

## Building a list

```java
ConditionList gate = ConditionList.builder()
        .require(PlaceholderCondition.parse("%player_level% >= 30"), Text.mini("<red>Level 30 required"))
        .require(PlaceholderCondition.parse("%vault_eco_balance% >= 500"), Text.mini("<red>You need 500 coins"))
        .requireSilent(PlaceholderCondition.parse("%player_world% == world"))
        .runCommands(PlaceholderCondition.parse("%player_afk% == true"),
                List.of("[console] kick %player_name% AFK"))
        .add(condition, FailurePolicy.STOP_CHAIN, Text.mini("<red>Blocked"))
        .build();
```

## Failure policies

The policy is attached per condition, so one list can mix a silent optional check with a loud
required one.

| Policy | Message | Cancels | Stops the chain | Runs commands |
|---|---|---|---|---|
| `SEND_MESSAGE` | Yes | No | No | No |
| `CANCEL` | Yes | Yes | No | No |
| `SILENCE` | No | No | No | No |
| `STOP_CHAIN` | Yes | No | Yes | No |
| `RUN_COMMANDS` | No | No | No | Yes |

`SEND_MESSAGE` is the default and keeps evaluating, so a player learns every reason they were
refused at once rather than one per attempt. `STOP_CHAIN` is for a gate that makes every later check
meaningless.

## The request

```java
ConditionRequest request = ConditionRequest.builder(resolver)
        .player(player)
        .actor(customActor)
        .consoleSink(consoleSink)
        .playerSink(playerSink)
        .build();

gate.test(request);

request.errors();        // every recorded failure message
request.isCancelled();   // whether a CANCEL policy fired
```

`ConditionRequest.forPlayer(player)` is the short form.

The request carries the error sink, so after `test` returns you have the list of messages to show,
not just a boolean.

`actor` is for gating something that is not a player: a block, an NPC, a scheduled job.

## Operands and operators

```java
PlaceholderCondition.of("%player_level%", Operator.GREATER_OR_EQUAL, "30");
PlaceholderCondition.parse("%player_level% >= 30");
```

| Symbol | Meaning |
|---|---|
| `==` | Equal; numeric when both sides are numbers, otherwise exact string equality |
| `!=` | Not equal |
| `>=`, `<=`, `>`, `<` | Numeric ordering; a non-numeric operand makes it false |
| `?=` | The left contains the right, as a substring |
| `*` | Glob match; `*` is any run and `?` is one character, matched in full |
| `\|\|` | Alternation; the right is a `\|`-separated list and the left must equal one branch |

The parser tries symbols longest first, so `>=` is found before `>` and the glob `*` is tried last.

`Comparison.parse(expression)` gives you the parsed pieces when you want to inspect them, and
`Comparison.of(operator).test(left, right)` is the bare comparison with no placeholders involved.

## Resolving operands

```java
OperandResolver resolver = (request, template) -> Placeholders.apply(request.player().orElse(null), template);
```

A two-argument function. This is the whole dependency on a placeholder engine, and it is yours to
supply: PlaceholderAPI in production, a map in a test.

## Custom conditions

`Condition` is a one-method interface, so a check that is not expressible as an operand comparison is
still a first-class entry in the list.

```java
Condition inRegion = request -> request.player()
        .map(p -> regions.canBuild(p, p.getLocation()))
        .orElse(false);
```
