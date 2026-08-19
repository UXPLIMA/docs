---
title: Text
order: 212
description: MiniMessage as the only text representation, typed placeholder sets, and downgrading a message for a client that cannot render it.
icon: type
---

All text in uxmLib is Adventure components built from MiniMessage. Legacy section-sign and ampersand
colour codes are deliberately unsupported, not missing.

## Text

`Text` is the single place a MiniMessage string becomes a component.

```java
Component title = Text.mini("<gradient:#ff5555:#ffaa00>Welcome</gradient>");
Component greet = Text.mini("<gray>Hello, <player>!", Text.placeholder("player", name));

String plain = Text.plain(title);       // formatting stripped, for logs
String source = Text.serialize(title);  // back to MiniMessage
String bare = Text.stripTags(input);    // remove tags from a raw string
```

| Resolver | Substitutes |
|---|---|
| `Text.placeholder(key, value)` | A literal string; tags inside it are not parsed |
| `Text.parsed(key, value)` | A string that is itself parsed as MiniMessage |
| `Text.component(key, value)` | A component |

The difference between `placeholder` and `parsed` matters. Player-supplied text goes through
`placeholder`, so a player cannot inject a click event into your message by typing one.

## Typed placeholder sets

`Placeholders<T>` binds a set of named placeholders to a subject type once, so a template can be
rendered for any subject without rebuilding resolvers at the call site.

```java
Placeholders<Player> player = Placeholders.<Player>builder()
        .add("name", Player::getName)
        .add("world", p -> p.getWorld().getName())
        .addParsed("prefix", p -> prefixOf(p))
        .addComponent("display", Player::displayName)
        .build();

Component line = player.render("<prefix><name> <gray>in <world>", viewer);
TagResolver resolver = player.resolver(viewer);
```

`add` is literal, `addParsed` parses the value as MiniMessage, `addComponent` supplies a component
directly. The same three-way split as `Text`, for the same reason.

## Rendering for the client that will read it

A Bedrock player has no gradients and no custom fonts. An older protocol cannot render an item
hover. The usual answers are to not use those features, or to send them anyway and let them break.

The third answer is to downgrade the message per recipient.

```java
ClientProfile profile = new ClientProfile(isBedrock, protocolVersion);
ClientCapabilities caps = ClientCapabilities.of(profile);

String adjusted = RenderDowngrade.apply(template, caps);
viewer.sendMessage(Text.mini(adjusted));
```

| Unsupported | Becomes |
|---|---|
| `<gradient:a:b>text</gradient>` | `<a>text</a>`, the first colour stop |
| `<font:...>text</font>` | `text`, the wrapper dropped |
| `<hover:show_item:'key':n>` | `<hover:show_text:'key xn'>` |

The downgrade is silent and string-level: the message always delivers, only the flourish degrades. A
fully capable client receives the input verbatim, because the matching pattern is skipped entirely
rather than applied and undone.

`ClientProfile.javaModern()` is the default for a normal Java client.

## Colour capability stripping

Where `RenderDowngrade` rewrites, `ColorCapabilityStripper` removes.

```java
String flat = ColorCapabilityStripper.strip(text, Set.of(ColorCapability.NAMED));
String bare = ColorCapabilityStripper.stripAllStyles(text);
boolean coloured = ColorCapabilityStripper.startsWithColourTag(text);
```

Useful when a sink genuinely cannot carry style at all, such as a plain-text log line or an external
API.

## Widths and centring

Minecraft's default font is not monospaced, so centring text means measuring it.

```java
int width = GlyphWidthTable.widthOf('W', false);
String centred = CenteredTextPadder.pad("Server Rules", true) + "Server Rules";
```

`CenteredTextPadder.pad` returns the leading spaces needed to centre a plain string, accounting for
bold. This is what makes a chat header line up instead of sitting slightly left.

## Two sanitisers

```java
LegacyCodeSanitizer.strip(text);         // remove section-sign codes from untrusted input
LangPrefixExpander.expand(template, prefixes);   // expand <prefix:name> style references
```

`LegacyCodeSanitizer` exists because uxmLib does not support legacy codes, so any that arrive from
outside should be removed rather than rendered literally.
