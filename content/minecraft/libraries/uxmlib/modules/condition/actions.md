---
title: Actions
order: 281
description: Config-driven action lists, parsed once into closures and run many times.
icon: circle-play
---

The other half of the pair. An operator writes a list of things to happen; you run it.

```java
ActionList actions = ActionList.parse(List.of(
        "[message] <green>Welcome back, %player_name%!",
        "[sound] minecraft:entity.player.levelup",
        "[console] give %player_name% diamond 1",
        "[title] <gold>Level up",
        "[close]"));

actions.run(context);
```

Parsing happens once. Running is invoking already-built closures, so a list attached to a hot event
does not re-parse per call.

## The action types

| Prefix | Effect | Payload |
|---|---|---|
| `[message]` | MiniMessage to the target audience | Required |
| `[broadcast]` | MiniMessage to every player | Required |
| `[actionbar]` | MiniMessage to the target's action bar | Required |
| `[title]` | MiniMessage as a title | Required |
| `[sound]` | Play an Adventure sound key to the target | Required |
| `[console]` | Dispatch a command from the console | Required |
| `[player]` | Dispatch a command as the target player | Required |
| `[close]` | Close the target's open inventory | None |

Prefixes are matched case-insensitively, so `[Message]` and `[MESSAGE]` both parse.

## The context

```java
ActionContext context = ActionContext.builder(resolver)
        .player(player)
        .target(player)
        .broadcast(Bukkit.getServer())
        .consoleSink(consoleSink)
        .playerSink(playerSink)
        .build();

context.resolve("%player_name%");
```

The two audiences are separate on purpose: `target` is who a `[message]` reaches, `broadcast` is who
a `[broadcast]` reaches. A gate evaluated for one player can still announce to the server.

## Command sinks

```java
CommandSink console = command -> scheduler.global(() -> Bukkit.dispatchCommand(Bukkit.getConsoleSender(), command));
```

A one-method seam. Commands never run on whatever thread evaluated the list; they go through the sink
you supply, which in production routes them through the library `Scheduler`. This is what makes the
engine Folia-safe and testable in the same stroke.

## Sync and async

```java
actions.hasAsyncActions();
actions.syncActions();
actions.asyncActions();
```

An `Action` declares whether it is async. Splitting the list lets a caller run the main-thread part
inline and dispatch the rest, rather than forcing everything onto one thread because one entry needs
network I/O.

## Building actions in code

```java
Actions.message("<green>Hello");
Actions.broadcast("<gold><player> won!");
Actions.actionBar("<yellow>Saved");
Actions.title("<gold>Level up");
Actions.console("give %player_name% diamond 1");
Actions.playerCommand("spawn");
Actions.close();
Actions.sound(new Actions.SoundSpec("minecraft:entity.player.levelup", 1f, 1f));

ActionList list = ActionList.of(List.of(Actions.message("<green>Hi"), Actions.close()));
```

The same closures the parser builds, so a code-defined list and a config-defined one behave
identically.

## Inspecting a parsed line

```java
ParsedAction parsed = ActionParser.parse("[message] <green>Hello");

parsed.type();       // ActionType.MESSAGE
parsed.payload();    // "<green>Hello"
parsed.action();     // the closure
```

Useful for validating an operator's config on load and reporting a bad line with its content, rather
than failing at the moment the action would have run.
