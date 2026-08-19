---
title: uxmlib-command
order: 24
description: A Brigadier facade and an annotation DSL on top of it, with typed arguments, flags, cooldowns, conditions and async handlers.
icon: terminal
---

Two ways to declare a command, both registering through Paper's Brigadier lifecycle.

The **annotation DSL** is the common case: a class, a method per branch, typed parameters. The
**`Cmd` facade** is there when you need to hand-build a node tree Brigadier-style.

They are not alternatives layered over different machinery. The annotation layer reflects a handler
into a platform-neutral command model and then renders that model into the same Brigadier tree you
would have written by hand.

| Page | Covers |
|---|---|
| [Annotation DSL](annotations.md) | `@Command`, `@Subcommand`, permissions, cooldowns, help, async |
| [Arguments](arguments.md) | Typed parameters, flags and switches, bounds, suggestions, custom resolvers |
| [Brigadier facade](brigadier.md) | `Cmd`, `Args`, `Sender`, `CommandRegistrar` |

## The short version

```java
@Command(name = "money", description = "Manage balances")
final class MoneyCommand {

    @Subcommand("pay")
    @Permission("money.pay")
    void pay(Sender sender, @Arg("target") Player target, @Arg("amount") @Range(min = 1) int amount) {
        sender.send(Text.mini("<green>Paid " + amount + " to " + target.getName()));
    }
}

AnnotatedCommands.register(plugin, new MoneyCommand());
```

A malformed handler fails at registration with a `CommandParseException`, not at the moment a player
runs it. Optional arguments that are not trailing, flags before positionals, a `@SuggestUsing` key
that was never registered: all caught on enable.
