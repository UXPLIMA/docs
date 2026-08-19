---
title: Configuration
order: 214
description: HOCON config with atomic reloads, live properties, migrations, validation, lenient lists and typed record snapshots.
icon: settings
---

Configuration over Configurate and HOCON, in two shapes: query individual nodes with `HoconConfig`,
or map a whole file onto one record with `RecordConfig`. Both reload atomically.

## HoconConfig

```java
HoconConfig config = HoconConfig.load(dataFolder.resolve("config.conf"));

int limit = config.getInt("homes.limit", 3);
String name = config.getString("server.name", "Server");
boolean flag = config.getBoolean("features.pvp", true);
```

A missing file loads as an empty tree, so every getter falls back to the default you pass and the
first run does not crash.

The parsed tree lives behind an atomic reference swapped whole on reload, so a reader always sees the
entire old tree or the entire new one. Never half.

### Shipping defaults

```java
HoconConfig config = HoconConfig.loadOrExtract(
        dataFolder.resolve("config.conf"), "config.conf", getClassLoader());

config.mergeDefaults("config.conf", getClassLoader());   // add keys a new release introduced
config.commentIfAbsent("homes.limit", "How many homes a player may set");
config.save();
```

`loadOrExtract` writes the bundled resource out on first run. `mergeDefaults` adds keys the user's
file does not have yet without touching the ones they edited, which is what an upgrade needs.

### Typed reads

```java
List<String> worlds = config.getList("worlds", String.class);
Map<String, Warp> warps = config.getSection("warps", Warp.class);
Settings settings = config.getNode("settings", Settings.class, Settings.DEFAULT);
Optional<Root> whole = config.get(Root.class);
```

`ConfigCodecs.bukkit()` adds scalar serializers for `Material`, `NamespacedKey` and `Color`, so those
map straight from a string:

```java
HoconConfig config = HoconConfig.load(path, ConfigCodecs.bukkit());
```

### Lenient reads

One malformed entry in a list of fifty should not lose the other forty-nine.

```java
LenientResult<List<Warp>> result = config.getListLenient("warps", Warp.class);

result.value();        // everything that parsed
result.skipped();      // a ConfigViolation per entry that did not
result.allParsed();    // whether anything was skipped
```

This is the right default for operator-edited collections. Log the skips and carry on with what
worked, rather than refusing to load.

### Live properties

```java
ConfigProperty<Integer> limit = config.intProperty("homes.limit", 3);
limit.onChange(value -> rebuildLimits(value));

config.reload();       // fires the listener only if the value actually changed
```

A property re-reads itself on reload and fires its listeners only when the value moved, so nothing
has to poll the file to find out something changed. There are `intProperty`, `boolProperty` and
`stringProperty` variants.

`config.onReload(runnable)` is the coarser hook for work that is not tied to one key.

### Watching the file

```java
config.watch(scheduler, Duration.ofSeconds(5));
config.unwatch();
```

Polls the file on a period and reloads when it changed, so an operator editing the file gets it
applied without a command.

### Migrations

```java
ConfigMigration migration = ConfigMigration.builder()
        .version(2, step -> step.rename("homes.max", "limit"))
        .version(3, step -> step.remove("legacy.section"))
        .build();

int applied = config.migrate(migration);
```

Each version's step runs once, in order, and the file records where it got to. `rename` and `remove`
are the two operations.

### Validation

```java
ValidationResult result = new ConfigRules()
        .require("database.url")
        .range("homes.limit", 1, 100)
        .matches("server.id", "[a-z0-9-]+")
        .oneOf("storage.type", "sqlite", "mysql", "postgres")
        .validate(config);

result.ok();
result.violations();          // a ConfigViolation per failure: path and message
result.throwIfInvalid();      // or fail loudly on enable
```

Validating on enable turns a config typo into one clear line at startup instead of a
`NumberFormatException` an hour later.

### Interpolation

```java
config.interpolate(Map.of("server", "lobby-1"));
String motd = config.getInterpolated("motd", "Welcome", Map.of("server", "lobby-1"));
```

Substitutes variables into string values, so one file can be shared across nodes with the node name
injected.

### Composition

```java
config.include(otherConfig);
config.include(dataFolder.resolve("extra.conf"));
```

Merges another tree in, for splitting a large config across files.

### Saving

```java
config.save();
config.saveAsync(executor);
```

Writes are atomic: the file is written beside the target and moved into place, so a crash mid-write
cannot leave a half-written config behind.

### Raw access

```java
config.nodeAt("menus", "shop");     // a ConfigurationNode
config.root();                       // the whole commented tree
config.keys("warps");                // the child keys under a path
```

`nodeAt` is what you hand to `MenuConfig.load` or any other node-consuming API.

## RecordConfig

When the whole file is one shape, map it onto one `@ConfigSerializable` record.

```java
RecordConfig<Settings> settings = new RecordConfig<>(
        dataFolder.resolve("settings.conf"), Settings.class, Settings::new);

Settings current = settings.current();     // a cached field read, cheap on a hot path
settings.reload();
settings.dryRun();                          // parse without swapping, to validate a pending edit
settings.isModifiedSinceLoad();             // has the file changed on disk
```

Reload is fail-safe: on a parse error it throws and **keeps the prior snapshot**, so `current()`
always returns a valid value. A bad edit does not take the server down with it.

`RecordConfig.loadFrom(file, type, default)` is the one-shot static when you do not need the
reloadable wrapper.

## Literal or placeholder

A config field that may be either a fixed value or a placeholder template resolved later.

```java
ValueOrPlaceholder<String> owner = ValueOrPlaceholder.placeholder("%player_name%");

String resolved = owner.resolve(template -> Placeholders.apply(player, template));
```

`ValueOrPlaceholder.literal("Steve")` ignores the resolver entirely. A placeholder calls it on every
resolve, never caching, so live values stay live. The resolver is a plain function, which is why this
type does not depend on any placeholder engine.

## Merging user edits

```java
Map<String, Value> merged = MapMerge.userWins(managedDefaults, userEdits);
```

For the case where you own a set of entries and the operator may override any of them.
