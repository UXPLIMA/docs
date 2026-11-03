---
title: Your first chat
order: 204
description: Pick the modules, shape the channels and the format, give ranks their tags, and check it in game.
icon: flag
---

A walk through the first ten minutes on a new server. Each step names the page that goes deeper.

## 1. Name the server and pick the modules

In `config.conf`, set `server-name` to the name this server goes by (on a network, its name in `velocity.toml`, in
lower case). Then turn off what you do not want under `modules`: a lobby might write `games = false`,
`mail = false`, `bubbles = false`. `/uxmchat reload`.

## 2. Look at the channels

`/channel list` shows `global`, `local`, `world`, `trade`, `help` and `staff`. Type `!hello` to speak in global from
any channel, `/l hi` to say one line in local, `/trade` to move to trade. To drop a channel, open
`/chateditor channels`, click it and delete it; to add one, click the lime dye. See [Channels](../features/channels.md).

## 3. Shape the line

Open `formats.conf`. The `global` format draws `[G] <tag><prefix><name>: <message>`. Change a part's template, its
hover or its click, then `/uxmchat reload`. With LuckPerms, `<prefix>` is the rank prefix. See
[Formats](../features/formats.md).

## 4. Give ranks their tags

The shipped `member`, `vip` and `staff` tags are held by the permission groups `default`, `vip` and
`helper`, `moderator`, `admin`, `owner`. A player who chose no tag wears the held one with the highest
`default-priority`. Rename the groups in `/chateditor tags` to match your ranks. See [Tags](../features/tags.md).

## 5. Let players colour their words

Every player may pick the five solid colours; give the VIP rank `uxmchat.colour.preset.sunset` and the other gradient
nodes. See [Chat colours](../features/colours.md).

## 6. Check it

Join as a player who is not an operator and type a line with a swear word, a link and `:heart:`. The swear word is
starred, the link line is refused, the heart is drawn. `/chat` opens the settings window, `/tag` the tag window,
`/chatcolor` the colour window.

## Next

- A network of servers: [A network](../features/network.md).
- Discord: [Discord](../features/discord.md).
- Moving from uxm-tags or uxm-chat-color: [Migration](../integrations/migration.md).

Related: [Concepts](concepts.md), [Configuration](../config/index.md)
