---
title: Brigadier facade
order: 243
description: Building a node tree by hand, reading arguments, and the sender wrapper.
icon: git-branch
---

When the annotation DSL does not fit, build the tree directly. The facade is thin on purpose: it
removes the boilerplate around Paper's Brigadier lifecycle without hiding Brigadier itself.

## Registering a tree

```java
CommandRegistrar.register(plugin,
        Cmd.literal("ping")
                .requires(Cmd.permission("example.ping"))
                .executes(ctx -> {
                    Sender.of(ctx.getSource()).send(Text.mini("pong"));
                    return Cmd.OK;
                }),
        "Replies with pong");
```

`CommandRegistrar.register` hooks Paper's command lifecycle event for you, which is the part that is
easy to get wrong and boring to repeat. There is an overload taking aliases.

## Building nodes

```java
Cmd.literal("home");
Cmd.argument("name", StringArgumentType.word());
Cmd.permission("home.use");     // a Predicate<CommandSourceStack> for requires
Cmd.OK;                          // the success return value
```

## Reading arguments

```java
String name = Args.string(ctx, "name");
int amount = Args.integer(ctx, "amount");
double value = Args.number(ctx, "value");
boolean flag = Args.bool(ctx, "flag");
```

Thin typed reads over `CommandContext`, so a handler does not repeat the generic call shape.

## The sender wrapper

```java
Sender sender = Sender.of(ctx.getSource());

sender.send(Text.mini("<green>Done"));
sender.isPlayer();
Optional<Player> player = sender.player();
CommandSender bukkit = sender.bukkit();
CommandSourceStack source = sender.source();
```

`Sender` is what the annotation DSL injects too, so a handler written for one style reads the same in
the other.

## Mixing the two

```java
LiteralCommandNode<CommandSourceStack> node = AnnotatedCommands.buildNode(new HomeCommand());
```

Builds the annotated handler's tree without registering it, so you can graft it under a root you
built by hand. The annotation layer emits an ordinary Brigadier tree, so there is nothing special
about the node it hands back.
