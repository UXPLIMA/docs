---
title: Text input
order: 236
description: Asking a player to type a line, over anvil, chat or sign, behind one contract.
icon: keyboard
---

Three native backends, one callback shape. No packets and no NMS in any of them.

| Backend | How it captures text |
|---|---|
| `InputType.ANVIL` | The rename field of a vanilla anvil |
| `InputType.CHAT` | The player's next chat message |
| `InputType.SIGN` | A transient sign the player types into |

## Using it

Construct one per plugin, install it once, and open prompts as needed.

```java
PlayerInput input = new PlayerInput(plugin, scheduler, "cancel");
input.install();                       // onEnable

input.open(player, InputType.ANVIL, Text.mini("<gray>Enter a name"), result -> {
    switch (result) {
        case InputResult.Submitted submitted -> rename(submitted.text());
        case InputResult.Cancelled ignored -> reopenMenu(player);
    }
});

input.uninstall();                     // onDisable
```

`new PlayerInput(plugin)` is the short form; it uses the default cancel keyword `cancel` and no
scheduler.

`InputResult` is a sealed pair of `Submitted(String text)` and `Cancelled`, so the switch above is
exhaustive and the compiler tells you when you forgot a branch.

## Why one contract matters

Each backend fails differently on different clients. An anvil prompt is invisible to a player who has
an inventory open elsewhere; a sign prompt behaves differently on Bedrock; a chat prompt is
intercepted by chat plugins. Switching a prompt from an anvil to a sign should be a parameter, not a
rewrite, and here it is one.

## What the router handles

- **A cancel keyword** aborts any backend. It defaults to `cancel` and is set in the constructor.
- **Pending state is per instance**, held in an `InputRouter`. There is no static mutable state, so
  two plugins prompting the same player do not collide.
- **Quit cleans up.** A player who logs out mid-prompt leaves nothing behind.
- **Thread marshalling.** The chat backend fires on an async event. When you pass a `Scheduler`, the
  result is marshalled back onto the player's region thread before your callback runs, so the
  callback may touch the Bukkit API safely.

<Callout type="warning" title="Without a Scheduler, a chat callback runs async">

The `PlayerInput(Plugin)` constructor has no scheduler, so a `CHAT` result reaches your callback on
the async chat thread. Pass a `Scheduler` if the callback touches anything Bukkit.

</Callout>

## The anvil backend on its own

`AnvilInput` is usable directly when the anvil is the only backend you want and you need control over
the prompt item.

```java
AnvilInput anvil = new AnvilInput(plugin);
anvil.install();

anvil.open(player, ItemBuilder.of(Material.PAPER).name(Text.mini("Type a name")).build(), result -> {
    if (result instanceof AnvilResult.Submitted submitted) {
        rename(submitted.text());
    }
});
```

The prompt item is what the player sees in the first anvil slot, and its display name is the text
they start editing.

## Choosing a backend

| Situation | Backend |
|---|---|
| Short value, player is already in a menu | `ANVIL` |
| Longer text, or the player needs to see chat context | `CHAT` |
| Multi-line value, or an anvil would cost a level | `SIGN` |

For servers on 1.21.6 and newer, a native text dialog is a fourth option that is not an inventory at
all. See [Dialogs](dialogs.md).
