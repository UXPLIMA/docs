---
title: Annotation DSL
order: 241
description: Declaring a command as a class, with permissions, player gates, cooldowns, generated help and asynchronous handlers.
icon: at-sign
---

## The class and its branches

```java
@Command(name = "home", aliases = {"h"}, description = "Manage your homes")
final class HomeCommand {

    @Subcommand
    void root(Sender sender) { ... }                        // /home

    @Subcommand(value = "set", description = "Save a home")
    void set(Sender sender, @Arg("name") String name) { ... }   // /home set <name>

    @Subcommand("admin reload")
    void reload(Sender sender) { ... }                      // /home admin reload
}
```

`@Subcommand` takes a space-separated literal path beneath the root. An empty path makes the method
the root's own executor.

A leading `Sender` or `CommandSourceStack` parameter is injected. Everything after it that carries
`@Arg` becomes a typed Brigadier argument.

## Registering

```java
AnnotatedCommands.register(plugin, new HomeCommand());
AnnotatedCommands.register(plugin, new HomeCommand(), resolvers);
AnnotatedCommands.register(plugin, new HomeCommand(), resolvers, scheduler);
```

The `Scheduler` overload is required for asynchronous handlers.

`AnnotatedCommands.buildNode(handler)` returns the built `LiteralCommandNode` without registering it,
for when you want to graft it into a larger tree yourself.

## Permissions

```java
@Permission("home.use")           // on the class: guards every branch
final class HomeCommand {

    @Permission("home.admin")     // on a method: guards that branch
    @Subcommand("admin reload")
    void reload(Sender sender) { ... }
}
```

A permission becomes a Brigadier `requires` check, so an unauthorised player does not see the branch
in tab completion at all.

## Player-only branches

```java
@PlayerOnly
@Subcommand("set")
void set(Sender sender, @Arg("name") String name) { ... }
```

Console and command blocks get a clean denial instead of an error. A method that injects a `Player`
parameter is player-only automatically; the annotation is for methods that have no such parameter but
still must not run from console.

## Cooldowns

```java
@Cooldown("30s")
@Subcommand("rtp")
void randomTeleport(Sender sender) { ... }
```

Keyed by the command path and the player's UUID, so different branches and different players are
independent. An early attempt is vetoed with a message naming the time still left. The console has no
UUID and is never gated.

The duration is the human form: `30s`, `1h30m`, `2d`.

By default cooldowns live in memory. Supply a `CooldownStore` through the resolvers to persist them
across restarts:

```java
ParamResolvers resolvers = ParamResolvers.withDefaults()
        .cooldowns(new Cooldowns(System::currentTimeMillis, myStore));
```

## Generated help

`@Command(help = true)` is the default and adds a `help` subcommand listing the visible branches,
paginated.

```java
@Secret
@Subcommand("debug")
void debug(Sender sender) { ... }
```

`@Secret` keeps a branch out of the listing. It still runs for anyone who types it and holds the
permission.

<Callout type="warning" title="Secret is not a security boundary">

It is a help-listing concern. A branch you genuinely want hidden needs a `@Permission`, because
Brigadier's `requires` is what keeps it out of tab completion too.

</Callout>

## Overlapping branches

Two branches can both match the same input, for instance `/give <amount>` taking an int and
`/give <name>` taking a string. Brigadier tries sibling argument nodes in attachment order and runs
the first that parses.

```java
@CommandPriority(1)
@Subcommand("give")
void giveAmount(Sender sender, @Arg("amount") int amount) { ... }

@CommandPriority(5)
@Subcommand("give")
void giveNamed(Sender sender, @Arg("name") String name) { ... }
```

A lower number is tried first. Unmarked branches rank last. This only matters where branches genuinely
overlap; distinct literal paths never collide.

## Asynchronous handlers

Return a `CompletableFuture` and the framework handles the completion for you.

```java
@Subcommand("stats")
CompletableFuture<Void> stats(Sender sender, @Arg("player") String name) {
    return profiles.loadAsync(name).thenAccept(profile -> sender.send(render(profile)));
}
```

The heavy work runs wherever you put it, typically `scheduler.async`. The framework's job is only to
make the **completion** safe: when the future settles it hops back through the library `Scheduler`,
onto the region owning the player who ran the command, or the global region for the console.

A future that completes exceptionally is reported on the same clean-error path a thrown handler uses,
not as a Brigadier stack trace in chat.

<Callout type="note" title="Async needs the Scheduler overload">

`AnnotatedCommands.register(plugin, handler, resolvers, scheduler)`. Without a scheduler there is
nothing to hop back onto.

</Callout>

## Pre-execute conditions

Brigadier's `requires` can only hide a node by permission. A condition is a runtime gate that
explains itself.

```java
ParamResolvers resolvers = ParamResolvers.withDefaults()
        .condition(context -> {
            if (inCombat(context)) {
                throw new CommandCondition.CommandConditionException("You are in combat.");
            }
        });
```

Every registered condition runs before arguments are bound. Throwing vetoes execution and sends the
reason to the sender in red, exactly like a rejected argument.

Use `@Permission` for anything that should also disappear from tab completion, and a condition for
anything that should explain why it said no.

## Reusable annotation shorthands

An `AnnotationReplacer` lets a project define its own annotation that expands into the library ones,
so a house convention is declared once.

```java
resolvers.replacer(AdminCommand.class, (annotation, element) -> List.of(
        Replacements.permission("myplugin.admin"),
        Replacements.marker(PlayerOnly.class)));
```

`Replacements.permission`, `Replacements.cooldown`, `Replacements.marker` and `Replacements.of` build
the synthetic annotations to return.
