---
title: config.yml
order: 501
description: Claim rules, integrations, borders, claim blocks, holograms and the database.
icon: file-cog
---

Seven sections. Every key below is shown with the value the plugin ships.

## claimSettings

| Key | Default | What it does |
|---|---|---|
| `expireMode` | `true` | Delete claims when their lifetime runs out |
| `expireTime` | `2592000` | Lifetime of a new claim, in seconds (30 days) |
| `minDistance` | `2` | Chunks of empty land required between different players' claims |
| `defaultBlock` | `'block1'` | Which `blocks` style a new claim uses |
| `defaultName` | `'Claim of %player.name%'` | Name given to an unnamed claim |
| `defaultFlags` | 13 flags | Flags a new claim starts with |

The shipped `defaultFlags` are `SPAWN_ANIMALS`, `SPAWN_MONSTERS`, `PISTON_PUSH`, `FLUID_FLOW`,
`TNT_EXPLOSIONS`, `CREEPER_DAMAGE`, `WITHER_DAMAGE`, `FIRE_SPREAD`, `REDSTONE`, `STRUCTURE_GROW`,
`NATURE_SPREAD`, `BLOCK_FORM` and `MOB_SPAWNING`.

That default set leaves explosions and fire **enabled** in new claims: the plugin ships permissive
and expects you to decide. A survival server that wants safe bases should remove `TNT_EXPLOSIONS`,
`CREEPER_DAMAGE`, `WITHER_DAMAGE` and `FIRE_SPREAD` before letting anyone claim.

`minDistance: 0` lets claims sit flush against each other, which is right for a city server and wrong
for a survival one.

## generalSettings

| Key | Default | What it does |
|---|---|---|
| `disabledWorlds` | `arena`, `spawn`, `world_nether` | Worlds where claiming is refused. **Case sensitive.** |
| `disabledCommandsInClaim` | `test`, `sethome`, `command` | Commands non-members may not run inside a claim |
| `licenseKey` | `'LICENSE_KEY_HERE'` | Your licence key |
| `smartCommand` | `true` | Make a bare `/claim` context aware |
| `economySupport` | `true` | Charge money through Vault |
| `economyProvider` | `''` | Pin a specific Vault provider or currency |
| `permissionSupport` | `true` | Read limit and ability nodes from the permission plugin |
| `worldGuardSupport` | `true` | Refuse claims overlapping a WorldGuard region |
| `placeholderSupport` | `true` | Parse placeholders in messages |
| `reminderEnabled` | `true` | Warn players about claims nearing expiry |
| `reminderOnLogin` | `true` | Warn them on join too |
| `reminderIntervalSeconds` | `300` | How often to check (5 minutes) |
| `reminderThresholdSeconds` | `86400` | How long before expiry to start warning (1 day) |

Edit `disabledWorlds` before opening the server. The shipped values are examples, and a world named
`world_nether` is disabled out of the box on most servers by accident.

`economyProvider` matches against the registered provider name, the Bukkit plugin name and the
currency names, singular and plural. When it is set and nothing matches, **startup fails** rather than
silently charging Vault's default economy, which is the correct behaviour for a multi-currency
server.

## border

The overlay `/claim showborders` draws.

| Key | Default | What it does |
|---|---|---|
| `ownClaimColor` | `'#24FF00CB'` | Colour for your own claims |
| `enemyClaimColor` | `'#FC0606CB'` | Colour for other players' claims |
| `wildernessColor` | `'#15BDEFB3'` | Colour for unclaimed land |
| `autoCloseSeconds` | `30` | Seconds before the overlay closes itself; `0` never closes it |
| `visualizerType` | `'DISPLAY_ENTITY'` | `DISPLAY_ENTITY` or `PARTICLE` |

Colours are `#RRGGBBAA`: eight digits, the last two being opacity. `#FF0000FF` is solid red,
`#FF000080` is half-transparent red.

`DISPLAY_ENTITY` is smoother and cheaper; `PARTICLE` is the fallback for clients or server versions
where display entities misbehave.

## blocks

Each entry defines a claim block style. Two ship.

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
| `block` | The material actually placed in the world |
| `icon.type` | The material shown in the style menu |
| `icon.glow` | Enchantment glint on the menu item |
| `icon.name`, `icon.lore` | Menu text |
| `icon.model` | Custom model data |

`icon.type` and `block` are separate so a style can be placed as bedrock but displayed as something
prettier in the menu.

## visualizer

Web-map integration.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Draw claims on the map |
| `style.borderColor` | `"#33FF33"` | Outline colour |
| `style.borderOpacity` | `0.8` | Outline opacity, 0–1 |
| `style.borderWeight` | `2` | Outline thickness |
| `style.fillColor` | `"#33FF33"` | Fill colour |
| `style.fillOpacity` | `0.35` | Fill opacity, 0–1 |
| `tooltip.lines` | 5 lines of HTML | The hover popup |

Tooltip lines are HTML, not MiniMessage: they are rendered by the map's web front end. Placeholders
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
| `textOpacity` | `0.98` | Text transparency |
| `lines` | 9 lines | The text itself, MiniMessage, with placeholders |

No claim block means no hologram: placing one is how a player opts in.

## database

| Key | Default | What it does |
|---|---|---|
| `type` | `'AUTO'` | `AUTO` infers the dialect from the URL |
| `username` | `'root'` | Ignored by SQLite |
| `password` | `'password'` | Ignored by SQLite |
| `url` | `'jdbc:sqlite:./plugins/uxmClaims/data/claims.db'` | The JDBC URL |

See [Database](../database/) for the three backends and when to move between them.

<Callout type="warning" title="Change the shipped database password">

The file ships with `username: 'root'` and `password: 'password'`. They are inert under SQLite, which
is the default URL, but a server that switches to MySQL without editing them is one misconfigured
firewall away from a bad day.

</Callout>
