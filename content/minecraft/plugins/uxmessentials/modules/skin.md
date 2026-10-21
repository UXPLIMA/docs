---
title: Skin
order: 9235
description: Player skins by name, by link or from a file, with Bedrock and cracked servers covered.
icon: shirt
---

Skin gives every player a face without a second plugin. They take one by name (`/skin Notch`), from an image on
the web (`/skin url <link>`) or from one you dropped on the server (`/skin file <name>`), and drop it again with
`/skin clear`. Nobody has to type anything for the common case: a cracked server dresses a paid account in its
real skin, a Bedrock player arrives wearing their Bedrock skin, and everyone else is dressed from a pool you
choose instead of appearing as Steve.

Module `skin` · disabled by default · `modules/skin/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/skin` (`/skins`) | Wear the skin of another account, an image on the web, or one of the server's own. | `uxmessentials.skin.use` |
{/* /generated */}

`/skin` carries its own subcommands: `set <name> [player]`, `url <link> [slim]`, `file <name> [slim]`, `clear
[player]`, `update`, `drop <player>`, `info <player>` and `purge <name>`. `/skin <name>` is the short form of
`set`.

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.skin` | op | Hot-reload / inspect the skin module (the skin a player wears). |
| `uxmessentials.skin.drop` | op | /skin drop \<player>: delete a stored skin, so their next join derives one afresh. |
| `uxmessentials.skin.file` | op | /skin file \<name>: wear one of the skins the operator dropped in the server's skin folder. |
| `uxmessentials.skin.info` | op | /skin info \<player>: which skin somebody wears, from where, and when it was set. |
| `uxmessentials.skin.name.<skin>` | everyone | Wear the skin of one named account; only checked for the skins the operator restricted. |
| `uxmessentials.skin.other` | op | /skin set \<name> \<player> and /skin clear \<player>: dress or undress somebody else. |
| `uxmessentials.skin.purge` | op | /skin purge \<name>: forget a cached texture so the next lookup goes back to the source. |
| `uxmessentials.skin.update` | everyone | /skin update: re-fetch your skin, for an account whose skin changed at the source. |
| `uxmessentials.skin.url` | everyone | /skin url \<link>: wear the skin drawn in an image published on the web. |
| `uxmessentials.skin.use` | everyone | /skin \<name> and /skin clear: wear another account's skin, or go back to your own. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `sources.name` | `true` | /skin \<name>: the skin worn by that account, resolved through Mojang |
| `sources.url` | `true` | /skin url \<link>: an image on the web, uploaded to MineSkin to be signed |
| `sources.file` | `true` | /skin file \<name>: an image the operator dropped into the folder named under mineskin below |
| `sources.bedrock` | `true` | a Bedrock player's own skin, when Floodgate is installed |
| `login.premium-skin` | `true` | dress a name that belongs to a paid account with the skin that account really wears |
| `login.default-pool` | `[]` | names used when nothing else resolves; a player keeps the same one on every join |
| `login.timeout-seconds` | `3` | how long a login lookup may take before the player is let in undressed |
| `bedrock.refresh-on-join` | `true` | look the skin up on every join, so a change made on the Bedrock side shows here |
| `bedrock.retries` | `2` | how many times a failed lookup is retried before the join falls through undressed |
| `limits.cooldown-seconds` | `30` | between one change and the next; 0 turns the cooldown off |
| `limits.blocked-skins` | `[]` | names nobody may wear, matched without regard to case |
| `limits.allowed-url-hosts` | `["i.imgur.com", "textures.minecraft.net"]` | hosts /skin url accepts; empty allows any host |
| `mineskin.api-key` | `""` | optional; without a key MineSkin rate-limits harder |
| `mineskin.folder` | `"skins"` | the folder under the plugin's data folder /skin file reads a \<name>.png from |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_skin_chosen%` | Whether the player chose a skin of their own (yes/no). |
| `%uxmessentials_skin_model%` | The player model the skin was cut for: classic or slim. |
| `%uxmessentials_skin_source%` | Where the player's chosen skin came from: by-name, by-url, by-file, bedrock or fallback. |
| `%uxmessentials_skin_value%` | What that source names: the account, the link, the file or the Bedrock id. |
{/* /generated */}

## Notes

- **The module ships off,** because an online-mode server already dresses everybody correctly and would gain
  nothing but extra traffic. Switch it on for a cracked server, a Bedrock-facing one, or one where players
  should be able to choose.
- **A join is dressed before the player exists.** The profile is edited during the login handshake, so there is
  no respawn, no flicker and nothing to re-send; a live `/skin` change goes through the player's own profile and
  the server re-sends them.
- **The join order stops at the first hit:** the player's stored choice, then their real premium skin, then
  their Bedrock skin, then one from `login.default-pool` picked from their UUID so they keep the same face.
- **Nothing costs a login.** Every lookup runs off the server thread and is waited on for at most
  `login.timeout-seconds`; a slow Mojang, a MineSkin outage or a missing Geyser endpoint lets the player in
  undressed rather than holding the connection.
- **A link is uploaded to MineSkin to be signed,** because an unsigned texture renders wrongly for other players
  on some clients. `limits.allowed-url-hosts` decides which hosts are accepted; leave it empty to accept any.
- **`/skin file` reads only from the configured folder.** A path that tries to leave it is refused without the
  file being read.
- **Bedrock skins come from Floodgate and the public Geyser skin service.** Without Floodgate the step resolves
  nothing and the join order moves on, so a Java-only server is unaffected.
- **A shared database is all cross-server sync needs:** the chosen skin is a row, so a player wears the same
  face on every server that reads the same database.

Related: [Player State](playerstate.md), [Presence](presence.md), [NPCs](npc.md)
