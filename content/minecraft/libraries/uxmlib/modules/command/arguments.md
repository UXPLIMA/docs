---
title: Arguments
order: 242
description: Typed parameters, flags and switches, bounds and validation, tab completion, and teaching the DSL a new type.
icon: braces
---

## The built-in types

A parameter's Java type drives the Brigadier argument type, so the client validates and tab-completes
it before the server sees it.

| Java type | Resolved from |
|---|---|
| `String` | A word, or the rest of the line when greedy |
| `int`, `long`, `double`, `float` | The matching native numeric type |
| `boolean` | A native boolean |
| `Player` | An online player, natively completed |
| `OfflinePlayer` | A player profile |
| `World` | A world |
| `Material` | The material registry |
| `Location` | A fine position, resolved against the sender's world |
| `Sound` | The sound-event registry |
| `UUID` | A UUID |
| Any `enum` | Its constants, listed as suggestions |

Being native matters: the client rejects a bad value locally, so a typo never reaches the server and
tab completion is free.

## Composed types

`List<T>`, `T[]` and `Optional<T>` compose over whatever element resolver the registry already knows.

```java
@Subcommand("give")
void give(Sender sender, @Arg("players") List<Player> targets) { ... }

@Subcommand("tag")
void tag(Sender sender, @Arg("name") String name, @Arg("note") Optional<String> note) { ... }
```

A `List<T>` or `T[]` reads one greedy trailing blob and maps every whitespace-separated token through
the element resolver, so a `List<World>` resolves each world exactly as a single `World` parameter
would. An `Optional<T>` is present only when a token was actually given.

Only a trailing parameter can be one of these, since each consumes a greedy node.

## Optional and greedy arguments

```java
@Subcommand("msg")
void message(Sender sender,
        @Arg("target") Player target,
        @Arg(value = "text", greedy = true) String text) { ... }

@Subcommand("home")
void home(Sender sender, @Arg(value = "name", optional = true, def = "default") String name) { ... }
```

An optional argument fills `def`, or the type's zero value, when absent. Only trailing arguments may
be optional, and only the last may be greedy. Both rules are checked at registration.

## Bounds

```java
@Arg("amount") @Range(min = 1, max = 64) int amount
@Arg("name") @Length(min = 3, max = 16) String name
```

`@Range` applies its bounds twice: the native Brigadier argument type is built with them so the
client rejects an out-of-range value before sending, and a server-side validator re-checks the
resolved value. A crafted packet or a non-Brigadier dispatch path cannot slip past.

`@Length` has no native equivalent, so it is enforced server-side after the word is parsed, on the
same clean-error path a bad argument uses.

`@Arg(min =, max =)` still works as a thin alias, but prefer `@Range` in new code.

## Flags and switches

Position-independent named arguments, given after the positional ones in any order.

```java
@Subcommand("ban")
void ban(Sender sender,
        @Arg("target") Player target,
        @Flag(value = "reason", shorthand = 'r') String reason,
        @Switch(value = "silent", shorthand = 's') boolean silent) { ... }
```

```
/ban Notch --reason "griefing" --silent
/ban Notch -r "griefing" -s
```

A `@Flag` takes a value and its Java type drives the parse, exactly like an `@Arg`. A `@Switch` is a
presence flag on a `boolean` and takes no value.

A flag is optional. When omitted, the parameter gets its type's zero value, or `null` for a reference
type, so prefer a boxed type or check for absence.

All flag parameters must come after the positional arguments. Checked at registration.

## Tab completion

```java
@Arg("mode") @Suggest({"on", "off"}) String mode

@Arg("warp") @SuggestWith(WarpSuggestions.class) String warp

@Arg("kit") @SuggestUsing("kits") String kit
```

| Annotation | Source |
|---|---|
| `@Suggest` | A fixed list of literals |
| `@SuggestWith` | A `SuggestionSource` class, instantiated once at registration through its no-arg constructor |
| `@SuggestUsing` | A `SuggestionSource` instance you built and registered by key |

`@SuggestUsing` is the dependency-injection-friendly one: `@SuggestWith` reflects a class through its
no-arg constructor and so cannot carry your collaborators, while `@SuggestUsing` references a fully
constructed provider.

```java
ParamResolvers resolvers = ParamResolvers.withDefaults()
        .suggestions("kits", new KitSuggestions(kitService));
```

A `@SuggestUsing` key that was never registered fails at registration with a `CommandParseException`.
A typo never silently disables completion.

## Teaching it a new type

```java
ParamResolvers resolvers = ParamResolvers.withDefaults()
        .register(Warp.class, new WarpResolver(warpService))
        .validate(Warp.class, warp -> warp.isEnabled() ? null : "That warp is disabled.")
        .context(Guild.class, context -> guildOf(context.getSource()));
```

| Method | Adds |
|---|---|
| `register(Class, ParamResolver)` | How to parse and complete a type |
| `factory(ParamResolver.Factory)` | A resolver derived from other resolvers, for generic shapes |
| `validate(Class, ParameterValidator)` | A post-parse check for every parameter of that type |
| `context(Class, ContextParameter)` | A parameter injected from the dispatch rather than parsed |
| `suggestions(key, SuggestionSource)` | A named completion provider |
| `condition(CommandCondition)` | A pre-execute gate |
| `cooldowns(Cooldowns)` | The cooldown store backing `@Cooldown` |
| `replacer(Class, AnnotationReplacer)` | An annotation shorthand |

A **context parameter** is the difference between every handler starting with three lines of lookup
and none of them doing so. A `Guild` parameter that is derived from the sender is injected, not
typed by the player.

There is no global mutable state here: the registry is an object you build and pass to
`AnnotatedCommands.register`, so two plugins never see each other's resolvers.
