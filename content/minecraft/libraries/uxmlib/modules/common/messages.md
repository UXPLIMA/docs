---
title: Messages and i18n
order: 213
description: A message catalog with three-tier locale fallback, and delivery channels an operator can retarget without touching code.
icon: languages
---

Two independent things are usually tangled together in a plugin: what a message says, and how it
reaches the player. This module separates them, so a translator edits text and an operator edits
delivery, neither touching code.

## Declaring keys

A `MessageKey` is a dotted path plus the default text baked into your code. Declaring them as an enum
keeps every path and its default in one auditable place.

```java
enum Msg implements MessageKey {
    WELCOME("join.welcome", "<green>Welcome, <player>!"),
    NO_PERMISSION("error.permission", "<red>You cannot do that.");

    private final String path;
    private final String defaultTemplate;

    Msg(String path, String defaultTemplate) {
        this.path = path;
        this.defaultTemplate = defaultTemplate;
    }

    @Override public String path() { return path; }
    @Override public String defaultTemplate() { return defaultTemplate; }
}
```

`MessageKey.of(path, default)` is the bare pair when an enum is overkill.

A plugin therefore never ships a message with no text: the default is the last-resort tier and the
seed an operator gets when no translation exists.

## Sending

```java
Messages messages = new Messages(catalog, LocaleSource.ofDefault(Locale.ENGLISH));

messages.send(player, Msg.WELCOME, Text.placeholder("player", player.getName()));
Component rendered = messages.render(player, Msg.WELCOME, resolvers);
```

`send` resolves the viewer's locale, looks up the template, renders it through MiniMessage with your
placeholders, and delivers it over the channel configured for that key. `render` stops short of
delivery when you want the component for something else.

## Three-tier fallback

`MessageCatalog` resolves a key to a template in this order:

1. The viewer's exact locale, then its language-only form, so `en_US` falls back to `en`.
2. The same lookup against the configured default locale.
3. The key's own `defaultTemplate()`.

It always returns text. A missing translation degrades a language at a time, not to an empty string.

```java
MessageCatalog catalog = MessageCatalogLoader.fromNodes(nodesByLocale, Locale.ENGLISH);

catalog.template(Msg.WELCOME, Locale.GERMAN);
catalog.isTranslated(Msg.WELCOME, Locale.GERMAN);   // true only for a real entry
```

`isTranslated` is what a `/lang status` command should report, because it distinguishes a real
translation from a fallback that happens to render.

## Seeding a lang file

```java
Map<String, String> starter = MessageCatalog.defaults(Msg.class);
```

Harvests every key's default into a `path -> template` map, ready to write out as a starter lang
file. An operator then edits real text rather than inventing paths.

## Retargetable channels

The same catalog value carries its template; the channel is a separate, admin-supplied map. Swapping
chat for a title is a config edit with zero code change.

```java
Messages messages = new Messages(catalog, locales, Map.of(
        "join.welcome", Message.chat("<green>Welcome"),
        "combat.tagged", new Message.ActionBar("<red>In combat")));
```

| Variant | Delivery |
|---|---|
| `Message.Chat` | `sendMessage` |
| `Message.TitleText` | Title and subtitle, with its own timings |
| `Message.ActionBar` | The action bar |
| `Message.BossBarText` | A boss bar |
| `Message.Silent` | Nothing is sent |

A key with no entry in the map is delivered as chat. `Message.Silent` is how an operator turns one
message off without deleting its text.

`MessageSerializer.collection()` reads and writes these from HOCON, so the channel map itself is a
config section rather than code.

## Locale sources

`LocaleSource` decides which locale a viewer gets.

```java
LocaleSource locales = LocaleSource.ofDefault(Locale.ENGLISH);
```

`ofDefault` is the production implementation: a `Player` contributes its own client locale, and
anything else uses the default you pass. A server that wants a single language regardless of client,
or a per-player override stored in a database, implements the two-method interface itself. It is also
the seam a test injects a fake through, so catalog behaviour is testable with no live server.
