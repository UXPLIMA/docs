---
title: Utilities
order: 216
description: Durations, number formatting, semantic and server versions, sound lookup, one-shot warnings and ReDoS-guarded regex.
icon: wrench
---

The small pieces every plugin ends up writing badly once.

## Durations

```java
Duration ttl = Durations.parse("1h30m");
Optional<Duration> maybe = Durations.tryParse(userInput);

Durations.format(Duration.ofSeconds(3725));        // exact
Durations.approximate(Duration.ofSeconds(3725));   // rounded, for a UI
```

`parse` throws on bad input, `tryParse` returns empty. Use `tryParse` for anything an operator or a
player typed.

`format` is for a countdown that must be exact. `approximate` is for "about 2 hours", which is what
you want on a boss bar that updates every second.

## Numbers

```java
Numbers.grouped(1234567);                 // 1,234,567
Numbers.grouped(1234567, Locale.GERMAN);  // 1.234.567
Numbers.abbreviate(1234567);              // 1.2M
Numbers.abbreviate(1234567, 2);           // 1.23M
Numbers.roman(14);                        // XIV
```

`grouped` respects the locale, which matters once you have a message catalog rendering per viewer.
`abbreviate` is for a scoreboard line with nine characters of room. `roman` is for tier and level
displays.

## Semantic versions

```java
SemanticVersion current = SemanticVersion.parse("1.4.2-beta.1");
Optional<SemanticVersion> maybe = SemanticVersion.tryParse(raw);

current.release();                      // 1.4.2, pre-release stripped
current.isNewerThan(other);
current.compareTo(other);
```

Full semver ordering, including pre-release precedence, which is what makes the update checker able
to say a stable release is newer than a beta of the same number.

## Server version

```java
ServerVersion version = ServerVersion.current();

version.isAtLeast(1, 21, 6);
version.isAtLeast(1, 21);
ServerVersion.isFolia();
```

This is how a feature gates itself cleanly rather than catching `NoClassDefFoundError`. The dialogs
facade in the GUI module uses exactly this.

## Sounds

```java
Optional<Sound> sound = Sounds.resolve("minecraft:block.note_block.pling");
```

Resolves a sound key from config text, returning empty rather than throwing when an operator typos
it. Pair it with a fallback so a bad key is a missing sound, not a broken feature.

## Warn once

A misconfiguration should be reported clearly, once, not every tick.

```java
WarnOnce warnings = new WarnOnce(logger::warning);

warnings.warn("missing-economy", "No economy plugin found; prices are disabled.");
warnings.hasWarned("missing-economy");
warnings.reset();
```

Keyed, so unrelated warnings do not suppress each other. `reset` on reload, so a fixed
misconfiguration can report again if it comes back.

## ReDoS-guarded regex

A regex from a config file is untrusted input. A pathological pattern against a long string can hang
a thread indefinitely, and the plugin that loaded it gets the blame.

```java
TimedRegex regex = new TimedRegex(executor, Duration.ofMillis(50), logger::warning);

boolean matched = regex.run("chat-filter", () -> pattern.matcher(message).find(), false);
```

The operation runs with a timeout and an interruptible input. When it overruns, the fallback is
returned, the id is logged, and the server keeps ticking.

`TimedRegex.interruptible(input)` wraps a `CharSequence` so the regex engine notices a thread
interrupt mid-match, which a plain `String` does not.

<Callout type="tip" title="Use it wherever a pattern comes from config">

Chat filters, name validation, command matchers. Anywhere the pattern is not a constant in your own
source, it is untrusted.

</Callout>

## Banner

```java
Banner.print("MyPlugin", "1.4.2", component -> getComponentLogger().info(component));
List<Component> lines = Banner.lines("MyPlugin", "1.4.2");
```

The uxmLib startup banner, exposed so a plugin built on it can print its own.
