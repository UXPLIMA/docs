---
title: uxmlib-command
order: 24
icon: terminal
---

Two ways to write a command, both registering through Paper's Brigadier lifecycle: an annotation DSL
for the common case, and a thin facade when you need to build the node tree yourself.

## The annotation DSL

```java
@Command(name = "money", description = "Manage balances")
class MoneyCommand {

    @Subcommand("pay")
    @Permission("money.pay")
    void pay(Sender sender, @Arg("target") String target, @Arg(value = "amount", min = 1) int amount) {
        sender.send(Text.mini("<green>Paid " + amount + " to " + target));
    }
}

AnnotatedCommands.register(plugin, new MoneyCommand());
```

Types on parameters drive parsing and tab completion: an `int` parameter is a Brigadier integer
argument with integer suggestions, a `Player` parameter completes online players.

## Annotations

| Annotation | Effect |
|---|---|
| `@Command` | Marks the class and names the root |
| `@Subcommand` | A branch under the root |
| `@Arg` | Names an argument and sets bounds |
| `@Permission` | Required permission |
| `@PlayerOnly` | Refuses console |
| `@Range`, `@Length` | Numeric and string bounds |
| `@Cooldown` | Per-sender rate limit |
| `@Flag`, `@Switch` | `--name value` and `--name` |
| `@Suggest`, `@SuggestWith`, `@SuggestUsing` | Completions: literal, provider class, method |
| `@Secret` | Hidden from generated help |

Bounds are enforced before your method runs, and the failure message is generated. There is no
argument validation to write and no chance of forgetting it on one branch.

## Async execution

A command can run off the main thread when its work is I/O. Declare it and the framework dispatches
accordingly, so a database lookup does not hold up a tick.

## Help

`HelpPages` and `HelpRenderer` generate paginated help from the registered model — the same
descriptions the annotations already carry. `@Secret` keeps staff-only branches out of it.

## The facade

```java
CommandRegistrar.register(plugin,
        Cmd.literal("ping").requires(Cmd.permission("x.ping"))
                .executes(ctx -> {
                    Sender.of(ctx.getSource()).send(Text.mini("pong"));
                    return Cmd.OK;
                }),
        "Replies with pong");
```

`Cmd`, `Args`, `Sender` and `CommandRegistrar` are a thin layer over Brigadier — enough to remove the
boilerplate, not enough to hide the tree. Use it when the shape of the command is unusual enough that
annotations fight you.

The two styles interoperate: the annotation layer builds a Brigadier tree through the same facade.

## Sender

```java
sender.send(Text.mini("<green>Done"));
Player player = sender.player();
```

Wraps the Brigadier source. `Sender` is what a command method takes rather than `CommandSender`,
which is what lets `@PlayerOnly` be a declaration rather than a check at the top of every method.

## Extension points

Three SPIs:

| SPI | For |
|---|---|
| `ParamResolver` | Turning an argument into your own type |
| `ParameterValidator` | Rules beyond `@Range` and `@Length` |
| `CommandCondition` | Gates beyond permissions — a state check, a world check |

A resolver for your plugin's `Home` type means every command taking a home gets parsing, validation
and completion from one registration.

## Cooldowns

```java
@Cooldown(seconds = 5)
```

Backed by `CooldownStore`, per sender per command. The refusal message comes from the message
catalog, so it is translatable like everything else.
