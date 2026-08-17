---
title: First plugin
order: 13
description: A menu opened from a command, using common, item, gui and command.
icon: play
---

A plugin that opens a menu from a command, using `common`, `item`, `gui` and `command`.

## onEnable

```java
public final class ExamplePlugin extends JavaPlugin {

    private Scheduler scheduler;

    @Override
    public void onEnable() {
        this.scheduler = new PaperScheduler(this);
        Guis.install(this, scheduler);
        AnnotatedCommands.register(this, new MenuCommand(scheduler));
    }
}
```

`Guis.install` with the `Scheduler` overload is what enables animated and auto-refreshing items. The
overload without it works, with those features inert.

`Scheduler` is built once and injected. uxmLib has no static mutable state and no service locator —
library types are plain objects you construct.

## The command

```java
@Command(name = "menu", description = "Opens the example menu")
final class MenuCommand {

    @Subcommand("open")
    @PlayerOnly
    void open(Sender sender) {
        Player player = sender.player();

        SimpleGui menu = Guis.gui()
                .title(Text.mini("<dark_aqua>Example"))
                .rows(3)
                .build();

        menu.filler().fillBorder(GuiItem.display(
                ItemBuilder.of(Material.GRAY_STAINED_GLASS_PANE)
                        .name(Component.empty())
                        .build()));

        menu.set(2, 5, GuiItem.button(
                ItemBuilder.of(Material.DIAMOND)
                        .name(Text.mini("<aqua>Click me"))
                        .lore(Text.mini("<gray>A button"))
                        .build(),
                event -> sender.send(Text.mini("<green>Clicked"))));

        menu.open(player);
    }
}
```

Three things worth noticing:

**Clicks are cancelled by default.** An unconfigured menu cannot leak items. You opt into
interaction — with `StorageGui`, or by allowing it explicitly — rather than remembering to cancel.

**Slots are 1-indexed row and column.** `menu.set(2, 5, ...)` is the middle of the second row, not
raw slot 13. The single-argument `set(int, ...)` takes a raw slot when you want one.

**All text is MiniMessage.** `Text.mini` is the only way text enters the library. There is no
`ChatColor`, no `§`, and no `&` translation — those are deliberately unsupported, not missing.

## Configuration

```java
HoconConfig config = HoconConfig.load(getDataFolder().toPath().resolve("config.conf"));
int limit = config.getInt("homes.limit", 3);

ConfigProperty<Integer> live = config.intProperty("homes.limit", 3);
live.onChange(this::rebuildLimits);
```

A `ConfigProperty` fires its listener when a reload changes the value, so nothing has to re-read the
file to find out.

## Storage

```java
Database db = Database.builder()
        .sqlite(getDataFolder().toPath().resolve("data.db"))
        .build();

new MigrationRunner(db).apply(List.of(
        new Migration(1, "init",
                "CREATE TABLE players (uuid TEXT PRIMARY KEY, coins INTEGER)")));

Sql sql = new Sql(db);
```

Migrations run once each, in order, and are recorded — so this block is safe to leave in `onEnable`
forever.

## What to read next

| You want | Page |
|---|---|
| Menus in depth | [uxmlib-gui](../modules/gui.md) |
| The command DSL | [uxmlib-command](../modules/command.md) |
| Items and heads | [uxmlib-item](../modules/item.md) |
| Config, text and scheduling | [uxmlib-common](../modules/common.md) |
| Databases and caches | [uxmlib-storage](../modules/storage.md) |
