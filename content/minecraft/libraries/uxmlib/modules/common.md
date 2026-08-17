---
title: uxmlib-common
order: 21
description: Text, the Folia-ready scheduler, typed config, i18n and the utilities.
icon: box
---

The foundation every other module depends on. Nothing here depends on anything internal.

## Text

`Text` is the single place a MiniMessage string becomes an Adventure `Component`.

```java
Component title = Text.mini("<gradient:#ff5555:#ffaa00>Welcome</gradient>");
Component greet = Text.mini("<gray>Hello, <player>!", Text.placeholder("player", name));

String plain = Text.plain(title);      // strip formatting, for logs
String mm    = Text.serialize(title);  // round-trip back to MiniMessage
```

Legacy `§` and `&` codes are not supported anywhere in the library. That is a decision, not a gap:
mixing legacy codes with components produces text that renders differently depending on which path
it took, and having exactly one representation removes a whole class of formatting bug.

## Scheduling

One `Scheduler` interface over Paper's four schedulers. Build it once, inject it everywhere, and
your plugin never touches `BukkitScheduler` — which is what makes it run unchanged on Folia.

```java
Scheduler scheduler = new PaperScheduler(plugin);

scheduler.global(() -> broadcast());
scheduler.regionLater(location, Duration.ofSeconds(2), () -> grow());
scheduler.entityTimer(player, Duration.ZERO, Duration.ofSeconds(1),
        handle -> { if (done) handle.cancel(); });
scheduler.async(() -> fetchFromApi());
```

Every method returns a cancellable `TaskHandle`. Repeating tasks receive their own handle, so a task
can stop itself without holding a field.

| Method family | Runs on |
|---|---|
| `global` | The global region — server-wide state |
| `region` | The region owning a location |
| `entity` | The region owning an entity, following it |
| `async` | Off the main thread |

Picking the right one is the whole of Folia compatibility. Touching a block needs `region`; touching
a player needs `entity`; touching the player list needs `global`.

## Configuration

Two shapes, both over Configurate with HOCON.

### Node-based

```java
HoconConfig config = HoconConfig.load(dataFolder.resolve("config.conf"));

int limit = config.getInt("homes.limit", 3);

ConfigProperty<Integer> live = config.intProperty("homes.limit", 3);
live.onChange(value -> rebuildLimits(value));
config.reload();
```

A `ConfigProperty` is a live handle: its listener fires on reload when the value actually changed.
Nothing has to poll or re-read to notice.

### Typed records

```java
RecordConfig<Settings> settings =
        new RecordConfig<>(dataFolder.resolve("settings.conf"), Settings.class, Settings::new);

Settings current = settings.current();
settings.reload();
```

The whole file maps onto one `@ConfigSerializable` record. `current()` returns a cached snapshot,
cheap enough for a hot path, and `reload()` swaps in a new one atomically — or keeps the previous
value if the new file does not parse.

That last part matters: a typo in a config file leaves the server running on the last good
configuration instead of taking it down.

Also here: `ConfigWatcher` for file-change reloads, `ConfigMigration` and `ConfigUpgrade` for moving
old files forward, and `ConfigInterpolation` for referencing one value from another.

## Messages and i18n

`MessageCatalog` is a MiniMessage-native message store with per-locale files. `MessageKey` identifies
a message, `Messages` resolves it against a `LocaleSource`, and prefixes are expanded by
`LangPrefixExpander`.

The point is that message text lives in a file the server owner edits, and code refers to keys.

## Utilities

| Type | Purpose |
|---|---|
| `Durations` | Parsing and formatting durations from config strings |
| `Numbers` | Parsing and formatting numbers safely |
| `Sounds` | Sound keys from config, with validation |
| `Particles`, `ParticleData` | Type-safe particle spawning — the right data type per particle |
| `SemanticVersion` | Version comparison |
| `ServerVersion` | What the server is running |
| `TimedRegex` | Regex with a ReDoS guard |
| `AtomicFiles` | Write-and-rename, so a crash mid-write cannot corrupt a file |
| `Ticks`, `TickClock` | Ticks and durations, and a clock tests can control |

`TimedRegex` exists because a regex from a config file is user input. A catastrophically
backtracking pattern in a hot path is a denial of service, and this bounds how long a match may run
rather than trusting the pattern.

`Particles` is typed because the vanilla particle API takes an `Object` whose required class depends
on the particle — a mismatch throws at runtime. Here it does not compile.
