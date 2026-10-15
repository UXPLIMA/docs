---
title: config.yml
order: 500
description: Language, claim rules, integrations, dialogs, Bedrock forms, borders, claim blocks, holograms and the database.
icon: file-cog
---

The main settings file. Every key below is shown with the value the plugin ships. A key you delete
falls back to a built-in default, which is not always the shipped value (`defaultName` and the
database `url` differ), so edit keys rather than removing them.

## language

| Key | Default | What it does |
|---|---|---|
| `language` | `'en'` | Language of the texts in every file: `en`, `tr`, `es`, `pt-BR`, `de`, `fr`, `ru` or `pl` |

Changing it and running `/claim reload` rewrites the texts you have not edited. See
[Languages](languages.md).

## claimSettings

| Key | Default | What it does |
|---|---|---|
| `expireMode` | `true` | Delete claims when their lifetime runs out |
| `expireTime` | `2592000` | Lifetime of a new claim, in seconds (30 days) |
| `minDistance` | `2` | Chunks of empty land required between different players' claims; `0` allows adjacent claims |
| `defaultBlock` | `'block1'` | Which `blocks` style a new claim uses |
| `defaultName` | `'Claim of %player.name%'` | Name given to a new claim; `%player.*%` placeholders work |
| `defaultFlags` | 13 flags | Flags a new claim starts with |

The shipped `defaultFlags` are `SPAWN_ANIMALS`, `SPAWN_MONSTERS`, `PISTON_PUSH`, `FLUID_FLOW`,
`TNT_EXPLOSIONS`, `CREEPER_DAMAGE`, `WITHER_DAMAGE`, `FIRE_SPREAD`, `REDSTONE`, `STRUCTURE_GROW`,
`NATURE_SPREAD`, `BLOCK_FORM` and `MOB_SPAWNING`. A flag in the list is allowed, so new claims accept
explosions and fire spread. A survival server that wants safe bases removes `TNT_EXPLOSIONS`,
`CREEPER_DAMAGE`, `WITHER_DAMAGE` and `FIRE_SPREAD` before anyone claims. The names are listed in
[Flags](../protection/flags.md); a misspelt name makes claim creation fail.

## vault

| Key | Default | What it does |
|---|---|---|
| `blacklistedMaterials` | `[ ]` | Bukkit material names that cannot be stored in the claim vault, case insensitive |

A refused item answers with `errorVaultBlacklistedItem`. Typical entries are `PLAYER_HEAD` and
`BUNDLE`.

## generalSettings

| Key | Default | What it does |
|---|---|---|
| `disabledWorlds` | `arena`, `spawn`, `world_nether` | Worlds where claiming is refused, case insensitive, with or without WorldGuard |
| `disabledCommandsInClaim` | `test`, `sethome`, `command` | Commands non-members may not run inside a claim |
| `licenseKey` | `'LICENSE_KEY_HERE'` | Your licence key |
| `smartCommand` | `true` | A bare `/claim` on unclaimed land extends an adjacent claim of yours or creates one; `false` opens the menu |
| `confirmationEnabled` | `true` | Ask for confirmation (menu or `/claim confirm`) before destructive actions; `false` runs them at once |
| `economySupport` | `true` | Charge money through Vault |
| `economyProvider` | `''` | Pin a specific Vault provider or currency |
| `permissionSupport` | `true` | Read ability, limit, cost and delay nodes through LuckPerms |
| `worldGuardSupport` | `true` | Refuse claims overlapping a WorldGuard region |
| `placeholderSupport` | `true` | Register the `%uxmclaims_...%` PlaceholderAPI expansion |
| `reminderEnabled` | `true` | Warn players about claims nearing expiry |
| `reminderOnLogin` | `true` | Warn them on join too |
| `reminderIntervalSeconds` | `300` | How often to check (5 minutes) |
| `reminderThresholdSeconds` | `86400` | How long before expiry to start warning (1 day) |
| `accessGuardEnabled` | `true` | Eject players who end up inside a claim they are banned from or lack `MOVE_INSIDE` in |
| `accessGuardIntervalTicks` | `5` | How often each player's position is checked, in ticks (20 ticks is 1 second) |

`disabledWorlds` is checked when a claim is created, when a claim block is placed and when a chunk is
added, and the [claim map](../menus/claim-map.md) shows those worlds' chunks as not claimable. The list
is read on each check, so `/claim reload` applies a change.

`disabledCommandsInClaim` matches the command label with or without the slash, and the namespaced
form too: `sethome` also blocks `/essentials:sethome`. Members, operators and holders of
`uxmclaims.admin` are never blocked.

`economyProvider` matches the registered provider name, the Bukkit plugin name and the currency names,
singular and plural. When it is set and nothing matches, startup fails rather than charging Vault's
default economy.

`permissionSupport` needs LuckPerms. When it is off, or LuckPerms is missing, every ability node
counts as granted and every limit, cost and delay stays at its `defaultValue` from
[entitlements.yml](entitlements-yml.md).

The access guard catches every way in: walking, vehicles, pistons, knockback, teleports and logging in
inside. Holders of `uxmclaims.bypass.enter` are exempt. Lower intervals enforce more tightly and cost
more work per player.

## dialog

Native dialog screens (Java Edition 1.21.6+) for typing names, confirming actions and choosing claim
time. Which screens use them is set per menu file, see [Menu layouts](menus.md).

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Use dialogs where the server and the client support them |
| `bedrockFallback` | `true` | Give Bedrock players the classic menus instead of dialogs when Bedrock forms are off or unavailable |
| `timeoutSeconds` | `300` | How long an open dialog waits for an answer; after that its buttons do nothing. Paper and Folia only: on Spigot a dialog waits until it is closed |

A player gets the classic anvil, chat or chest menu instead when the server is older than 1.21.6
(Spigot) or 1.21.7 (Paper, Folia), when the client is older through ViaVersion, and when the player
is on Bedrock. Leaving `enabled: true` is safe on any server version.

## bedrock

Native Bedrock forms for players joining through Geyser or Floodgate. Without either plugin nothing
changes.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Use forms for typing and confirming instead of the anvil, chat and chest menus |
| `menus` | `true` | Also show the menus (claim list, members, warps, flags and the rest) as button lists |

The button lists are styled in `menu/bedrock/<menu>.yml`. The vault always stays a chest. With
`menus: false`, Bedrock players get forms for typing and confirming but chest menus otherwise.

## border

The overlay `/claim showborders` draws.

| Key | Default | What it does |
|---|---|---|
| `ownClaimColor` | `'#24FF00CB'` | Colour for your own claims |
| `enemyClaimColor` | `'#FC0606CB'` | Colour for other players' claims |
| `wildernessColor` | `'#15BDEFB3'` | Colour for unclaimed land |
| `autoCloseSeconds` | `30` | Seconds before the overlay closes itself; `0` never closes it |
| `visualizerType` | `'DISPLAY_ENTITY'` | `DISPLAY_ENTITY` (text display entities) or `PARTICLE`; an unknown value falls back to `DISPLAY_ENTITY` |

Colours are `#RRGGBBAA`, the last two digits being opacity: `#FF0000FF` is solid red, `#FF000080`
half-transparent red.

## blocks

Each entry defines a claim block style. Two ship: `block1` (bedrock, open to everyone) and `block2`
(obsidian, glowing icon, permission `claim.blockchange.obsidian`).

```yaml
blocks:
  block1:
    permission: ''
    block: 'BEDROCK'
    icon:
      type: 'BEDROCK'
      glow: false
      amount: 1
      name: 'Bedrock'
      lore:
        - ''
        - 'Click to select this claim style'
      model: 0
```

| Key | What it does |
|---|---|
| `permission` | Node needed to use this style; empty means everyone |
| `block` | The material placed in the world |
| `icon.type` | The material shown in the style menu |
| `icon.glow` | Enchantment glint on the menu item |
| `icon.amount` | Stack size shown on the item |
| `icon.name`, `icon.lore` | Menu text |
| `icon.model` | Custom model data |

## visualizer

Web-map integration.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Draw claims on the map |
| `style.borderColor` | `"#33FF33"` | Outline colour |
| `style.borderOpacity` | `0.8` | Outline opacity, 0 to 1 |
| `style.borderWeight` | `2` | Outline thickness |
| `style.fillColor` | `"#33FF33"` | Fill colour |
| `style.fillOpacity` | `0.35` | Fill opacity, 0 to 1 |
| `tooltip.lines` | 5 lines of HTML | The hover popup |

Tooltip lines are HTML, not MiniMessage, because the map's web front end renders them. Placeholders
work inside them. See [Map plugins](../integrations/maps.md).

## hologram

The floating text above the claim block.

| Key | Default | What it does |
|---|---|---|
| `height` | `1.4` | Vertical offset from the block |
| `viewRange` | `16` | Blocks at which it becomes visible |
| `seeThrough` | `false` | Visible through solid blocks |
| `alignment` | `'CENTER'` | `LEFT`, `CENTER` or `RIGHT` |
| `billboard` | `'CENTER'` | `FIXED`, `HORIZONTAL`, `VERTICAL` or `CENTER` |
| `backgroundColor` | `'#000000'` | Background colour behind the text |
| `defaultBackground` | `true` | Use Minecraft's own text background |
| `lineWidth` | `200` | Width before wrapping |
| `scale` | `1.0` | Overall size |
| `shadowed` | `true` | Drop shadow on the text |
| `shadowRadius` | `0.2` | Shadow blur |
| `shadowStrength` | `1.0` | Shadow opacity |
| `textOpacity` | `0.98` | Text opacity, 0 to 1 |
| `lines` | 9 lines | The text itself, MiniMessage, with placeholders |

No claim block means no hologram: placing one is how a player opts in.

## database

| Key | Default | What it does |
|---|---|---|
| `type` | `'AUTO'` | `AUTO`, `MYSQL`, `POSTGRES` or `SQLITE`; `AUTO` reads the dialect from the URL |
| `username` | `'root'` | Ignored by SQLite |
| `password` | `'password'` | Ignored by SQLite |
| `url` | `'jdbc:sqlite:./plugins/uxmClaims/data/claims.db'` | The JDBC URL |

One server per database: a second server pointed at the same MySQL or PostgreSQL database is refused
at startup. Change the shipped `root` and `password` before switching away from SQLite. See
[Database](../database/) for the three backends.

## Notes

- **`/claim reload` does not apply everything.** `database`, `economySupport`, `economyProvider`,
  `permissionSupport`, `worldGuardSupport`, `placeholderSupport`, `visualizerType`,
  `visualizer.enabled`, `reminderIntervalSeconds` and `dialog.timeoutSeconds` are read once at
  startup. Restart after changing them.
- **New keys arrive with updates.** A release adds its new keys to your file and keeps your values;
  see [Configuration](index.md).

<Callout type="warning" title="The shipped disabledWorlds list includes the Nether">

The shipped `world_nether` entry refuses claims in the Nether on most servers. Edit the list before
you open the server, then run `/claim reload`.

</Callout>
