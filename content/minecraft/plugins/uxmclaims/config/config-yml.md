---
title: config.yml
order: 240
icon: file-cog
---

## How to Edit

1. Stop your server OR use the in-game reload command
2. Edit the file with a text editor (Notepad++, VSCode, etc.)
3. Save the file
4. Start your server OR run `/claim reload`

<Callout type="warning" title="Be Careful!">

YAML files are sensitive to spacing. Use spaces, not tabs. Keep the structure intact.

</Callout>

---

## Section by Section

### 📋 claimSettings

These control the core behavior of claims.

<Callout type="info" title="Time Extension Costs">

Time extension costs are configured in [entitlements.yml](entitlements-yml.md) using the `uxmclaims.cost.time` permission.

</Callout>

```yaml
  expireMode: true
```

#### expireMode

**What it does:** Determines if claims automatically delete when they expire.

| Value   | Effect                                    |
|---------|-------------------------------------------|
| `true`  | Claims are **deleted** when time runs out |
| `false` | Claims stay forever, no expiration        |

**Recommendation:** Set `true` to clean up abandoned claims.

---

```yaml
  expireTime: 2592000
```

#### expireTime

**What it does:** How long new claims last by default (in **seconds**).

| Value    | Duration          |
|----------|-------------------|
| 86400    | 1 day             |
| 604800   | 7 days (1 week)   |
| 2592000  | 30 days (1 month) |
| 31536000 | 365 days (1 year) |

**Default:** 2,592,000 seconds = 30 days

---

```yaml
  minDistance: 2
```

#### minDistance

**What it does:** Minimum chunk gap between different players' claims.

| Value | Effect                                 |
|-------|----------------------------------------|
| 0     | Claims can touch each other            |
| 1     | At least 1 empty chunk between claims  |
| 2     | At least 2 empty chunks between claims |

**Why use this?** Prevents players from camping right next to each other.

---

```yaml
  defaultBlock: 'block1'
```

#### defaultBlock

**What it does:** Which block style new claims use by default.

This refers to the block types defined in the `blocks:` section below.

---

```yaml
  defaultName: 'Claim of %player.name%'
```

#### defaultName

**What it does:** The automatic name for new claims.

**Placeholders:**

- `%player.name%` = Player's name

**Example result:** "Claim of Steve"

---

```yaml
  defaultFlags:
    - 'SPAWN_ANIMALS'
    - 'SPAWN_MONSTERS'
    - 'PISTON_PUSH'
    - 'FLUID_FLOW'
    - 'TNT_EXPLOSIONS'
    - 'CREEPER_DAMAGE'
    - 'WITHER_DAMAGE'
    - 'FIRE_SPREAD'
```

#### defaultFlags

**What it does:** Flags that are **enabled** on all new claims.

Add or remove flags from this list to change the defaults.

See the [Flags Reference](../protection/flags.md) for all available flags.

---

### 🌍 generalSettings

Server-wide settings.

```yaml
generalSettings:
  disabledWorlds:
    - 'arena'
    - 'spawn'
    - 'world_nether'
```

#### disabledWorlds

**What it does:** Worlds where claiming is NOT allowed.

Players cannot create claims in these worlds. Case-sensitive!

---

```yaml
  disabledCommandsInClaim:
    - 'sethome'
    - 'test'
```

#### disabledCommandsInClaim

**What it does:** Commands that non-members cannot use inside claims.

Great for preventing:

- `/sethome` inside someone's claim
- Warping or teleporting into restricted areas

---

```yaml
  economySupport: true
  permissionSupport: true
  worldGuardSupport: true
  placeholderSupport: true
  smartCommand: true
```

#### Integration Toggles

| Setting              | What It Does                            |
|----------------------|-----------------------------------------|
| `economySupport`     | Use Vault economy (charging for claims) |
| `permissionSupport`  | Use Vault permissions                   |
| `worldGuardSupport`  | Prevent claims in WorldGuard regions    |
| `placeholderSupport` | Enable PlaceholderAPI                   |
| `smartCommand`       | Smart `/claim` behavior (see below)     |

Set to `false` to disable any integration.

---

#### smartCommand

**What it does:** Controls how the `/claim` command behaves.

| Value   | Effect                                                |
|---------|-------------------------------------------------------|
| `true`  | `/claim` auto-claims or extends if standing on land  |
| `false` | `/claim` opens the main menu directly                 |

**Default:** `true`

---

### 🎨 border

Visual border settings (display entities).

```yaml
border:
  ownClaimColor: '#24FF00CB'
  enemyClaimColor: '#FC0606CB'
  wildernessColor: '#15BDEFB3'
  autoCloseSeconds: 30
  visualizerType: 'DISPLAY_ENTITY'
```

#### Colors

Colors are in HEX format with alpha: `#RRGGBBAA`

| Part | Meaning              | Range                        |
|------|----------------------|------------------------------|
| RR   | Red                  | 00-FF                        |
| GG   | Green                | 00-FF                        |
| BB   | Blue                 | 00-FF                        |
| AA   | Alpha (transparency) | 00 (invisible) to FF (solid) |

**Examples:**

- `#FF0000FF` = Solid red
- `#00FF0080` = Semi-transparent green

---

#### autoCloseSeconds

**What it does:** Time in seconds before border visualization automatically closes.

| Value | Effect                                      |
|-------|---------------------------------------------|
| `30`  | Visualization closes after 30 seconds       |
| `0`   | Never auto-close (stays until toggled off)  |

**Default:** 30 seconds

---

#### visualizerType

**What it does:** The type of visualizer used to display claim borders.

| Value            | Effect                                    |
|------------------|-------------------------------------------|
| `DISPLAY_ENTITY` | Uses text display entities (recommended)  |
| `PARTICLE`       | Uses particle effects                     |

**Default:** `DISPLAY_ENTITY`

**Note:** Display entities provide better performance and visibility compared to particles.

---


### 🧱 blocks

Claim boundary block styles.

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

#### Each Block Entry

| Setting      | What It Is                                           |
|--------------|------------------------------------------------------|
| `permission` | Permission needed to use this style (empty = anyone) |
| `block`      | The Minecraft block type                             |
| `icon.type`  | Material shown in menus                              |
| `icon.glow`  | If the item glows in menus                           |
| `icon.name`  | Display name                                         |
| `icon.lore`  | Description lines                                    |
| `icon.model` | Custom model data (for resource packs)               |

Add more blocks by copying the structure with new names (`block3`, `premium`, etc.)

---

### 🗺️ visualizer

Web map settings (Dynmap, BlueMap, etc.)

```yaml
visualizer:
  enabled: true
  style:
    borderColor: "#33FF33"
    borderOpacity: 0.8
    borderWeight: 2
    fillColor: "#33FF33"
    fillOpacity: 0.35
  tooltip:
    lines:
      - '<span style="font-weight:bold;">%claim.name%</span>'
      - '• Owner: %claim.owner.name%'
      - '• Chunks: %count:claim.chunks%'
```

#### enabled

Set `false` to disable map integration entirely.

#### style

Controls how claims look on the web map.

#### tooltip.lines

HTML text shown when hovering over a claim on the map.

---

### 📍 hologram

Settings for the floating text above the claim block.

```yaml
hologram:
  height: 1.4
  viewRange: 16
  seeThrough: false
  alignment: 'CENTER'
  billboard: 'CENTER'
  backgroundColor: '#000000'
  defaultBackground: true
  lineWidth: 200
  scale: 1.0
  shadowed: true
  shadowRadius: 0.2
  shadowStrength: 1.0
  textOpacity: 0.98
  lines:
    - '<gold><bold>★ %claim.name% ★</bold></gold>'
    - '<gray>Owner: <white>%claim.owner.name%</white></gray>'
```

| Setting      | What It Does                                   | Default |
|--------------|------------------------------------------------|---|
| `enabled`    | Shows or hides the hologram                    | `true` |
| `height`     | How high above ground                          | `2.5` |
| `viewRange`  | Max distance to see it                         | `15` |
| `seeThrough` | Visible through blocks (walls/terrain)         | `false` |
| `alignment`  | Text alignment (LEFT, CENTER, RIGHT)           | `CENTER` |
| `billboard`  | Rotation behavior (FIXED, VERTICAL, HORIZONTAL, CENTER) | `CENTER` |
| `backgroundColor` | Background color (Hex code)                 | `#000000` |
| `defaultBackground` | Use default minecraft background        | `true` |
| `lineWidth`  | Max width of a line in pixels                  | `200` |
| `scale`      | Scale of the hologram (x, y, z)                | `1.0` |
| `shadowed`   | Whether text has a shadow                      | `true` |
| `shadowRadius` | Radius of the shadow                         | `5` |
| `shadowStrength` | Strength of the shadow (0.0 - 1.0)         | `0.5` |
| `textOpacity`| Opacity of the text (-1 for default)           | `-1` |
| `lines`      | Text to display (with colors and placeholders) | (see above) |

#### 🎨 Color Formats (RGB & RGBA)

Colors are defined using **HEX codes** (Hexadecimal). This allows for precise color selection using varying amounts of Red, Green, and Blue.

**Format Structure:** `#RRGGBB[AA]`

- **RR** = Red (00-FF)
- **GG** = Green (00-FF)
- **BB** = Blue (00-FF)
- **AA** = Alpha/Transparency (00-FF) *(Optional)*

##### 1. Solid Colors (RGB)
Standard 6-digit codes. These are fully opaque (solid).
*   `#FF0000` - Red
*   `#00FF00` - Green
*   `#0000FF` - Blue
*   `#FFFFFF` - White
*   `#000000` - Black

##### 2. Transparent Colors (RGBA)
8-digit codes. The last two digits (`AA`) control how see-through the color is.

| Opacity | Alpha Code (AA) | Example (Black) | Result |
| :--- | :--- | :--- | :--- |
| **100%** (Solid) | `FF` | `#000000FF` | Solid Black |
| **75%** | `BF` | `#000000BF` | Dark Shadow |
| **50%** | `80` | `#00000080` | Semi-Transparent |
| **25%** | `40` | `#00000040` | Light Shadow |
| **0%** (Invisible) | `00` | `#00000000` | Invisible |

> **Tip:** You can use online "HEX Color Pickers" to find the 6-digit code, then simply add the 2-digit Alpha code to the end to make it transparent.

#### 👁️ Visibility Settings

- **seeThrough:**
    - `true`: The hologram is visible through walls and terrain (like X-Ray/Glowing).
    - `false`: The hologram is hidden behind blocks (standard behavior).
- **shadowed:** Adds a drop shadow to the text for better readability against bright backgrounds.
- **textOpacity:** Controls the transparency of the text characters themselves (independent of background). `-1` uses the Minecraft default.

---

### 💾 database

Database connection settings.

```yaml
database:
  type: 'AUTO'
  username: 'root'
  password: 'password'
  url: 'jdbc:sqlite:./plugins/uxmClaims/claims.db'
```

#### type

- `AUTO` - Detect from URL (recommended)
- `MYSQL` - Force MySQL
- `SQLITE` - Force SQLite
- `POSTGRESQL` - Force PostgreSQL

#### url Examples

| Database   | URL                                          |
|------------|----------------------------------------------|
| SQLite     | `jdbc:sqlite:./plugins/uxmClaims/claims.db`  |
| MySQL      | `jdbc:mysql://localhost:3306/uxmclaims`      |
| PostgreSQL | `jdbc:postgresql://localhost:5432/uxmclaims` |

See [Database Setup](../database/sqlite.md) for detailed instructions.

---

## Full Example

Here's a complete `config.yml` with recommended settings for a typical server:

```yaml
claimSettings:
  expireMode: true
  expireTime: 2592000  # 30 days
  minDistance: 1
  defaultBlock: 'block1'
  defaultName: 'Claim of %player.name%'
  defaultFlags:
    - 'SPAWN_ANIMALS'
    - 'SPAWN_MONSTERS'
    - 'PISTON_PUSH'
    - 'FLUID_FLOW'
    - 'TNT_EXPLOSIONS'
    - 'CREEPER_DAMAGE'
    - 'WITHER_DAMAGE'
    - 'FIRE_SPREAD'
    - 'REDSTONE'
    - 'STRUCTURE_GROW'
    - 'NATURE_SPREAD'
    - 'BLOCK_FORM'
    - 'MOB_SPAWNING'

generalSettings:
  disabledWorlds:
    - 'spawn'
    - 'arena'
  disabledCommandsInClaim:
    - 'sethome'
  economySupport: true
  permissionSupport: true
  worldGuardSupport: true
  placeholderSupport: true
  smartCommand: true

border:
  ownClaimColor: '#24FF00CB'
  enemyClaimColor: '#FC0606CB'
  wildernessColor: '#15BDEFB3'
  autoCloseSeconds: 30
  visualizerType: 'DISPLAY_ENTITY'

database:
  type: 'AUTO'
  username: 'root'
  password: 'yourpassword'
  url: 'jdbc:sqlite:./plugins/uxmClaims/data/claims.db'
```

---

## Next Steps

- [📖 roles.yml](../config/roles-yml.md) - Configure default roles and permissions
- [💰 entitlements.yml](../config/entitlements-yml.md) - Set limits and costs
