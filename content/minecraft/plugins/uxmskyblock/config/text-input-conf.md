---
title: text-input.conf
order: 604
description: How a menu asks a player to write something.
icon: keyboard
---

Text input: how a menu asks a player to write something.

A menu asks for a word in six places: a warp's name, an island to visit, the island's name, a home's name, a player to invite, and an amount for the island bank. Each one captures the line in one of four ways:

```text
anvil  : a small anvil opens. The player writes in the rename field and takes the result.
chat   : the menu closes and the player's next chat message is the answer.
sign   : a sign opens for editing, and the line written on it is the answer.
dialog : a native dialog screen opens. A server too old for one uses a sign, then an anvil, and
         says so once in the console.
```

Pick one for every place below, or one for all of them with default-mode. Every place works in every way, so changing one is always safe. A Bedrock player always gets a native form instead, whichever way a place is set to, because an anvil or a sign does not work properly there.

The file is `plugins/uxmSkyblock/text-input.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `default-mode` | `anvil` | The way a place not listed under modes asks. anvil, chat, sign or dialog. |
| `cancel-keywords` | `cancel`, `iptal` | Writing one of these instead of an answer cancels the question and opens the menu again. The first one is the word the chat hint tells the player to write. |

## `modes`

One line for each place a menu asks. Delete a line and that place follows default-mode.

| Key | Default | What it does |
|---|---|---|
| `warp.name` | `anvil` |  |
| `island.visit` | `anvil` |  |
| `island.name` | `anvil` |  |
| `home.name` | `anvil` |  |
| `member.name` | `anvil` |  |
| `bank.amount` | `chat` |  |
| `role.name` | `anvil` |  |
| `guestbook.message` | `chat` |  |
| `alliance.player` | `anvil` |  |
