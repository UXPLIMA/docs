---
title: Settings
order: 7
description: The five config files, the kill switch, data modes, and the folder layout.
icon: sliders-horizontal
---

## The config files

| File | Holds | Replicated |
|---|---|---|
| `Configuration/Shared/Settings.luau` | The system switches | Yes |
| `Configuration/Shared/TrollSettings.luau` | The trolls themselves | Yes |
| `Configuration/Shared/DataSettings.luau` | Where player data lives | Yes |
| `Configuration/Shared/MarketplaceSettings.luau` | Purchase handling | Yes |
| `Configuration/Server/ImmunitySettings.luau` | Who is untouchable | No |
| `Configuration/Server/Hooks.luau` | Your code | No |

<Callout type="danger" title="Anything secret belongs in Configuration/Server">

`Configuration/Shared` is copied into `ReplicatedStorage`, so every client can read all of
it. An admin list, a webhook URL or an API key placed there can be read by any player.

The two files under `Configuration/Server` never leave the server. That is why immunity
lives there.

</Callout>

## Settings

```lua
Enabled = true,
Debug = true,
LogPrefix = "",
Security = { MaxRequestsPerSecond = 10 },
```

| Field | Default | What it does |
|---|---|---|
| `Enabled` | `true` | The kill switch |
| `Debug` | `true` | Informational messages in Output |
| `LogPrefix` | `""` | The tag on every log line. Empty uses the package name |
| `MaxRequestsPerSecond` | `10` | Per-player request cap |

### The kill switch

```lua
Enabled = false,
```

Nothing is installed, no remote is created, no service runs. The package sits in
`ServerScriptService` doing nothing.

That is the switch to reach for during an incident, rather than deleting the package and
losing your settings with it.

### Debug

Ships as `true`. Turn it off before you go live: it prints informational messages for
routine activity. Warnings and errors are always shown either way.

### The rate limit

```lua
MaxRequestsPerSecond = 10,
```

Requests above this from a single client are dropped silently. It is exploit protection, not
a game rule: normal play stays well below it, so lowering it is safe.

The game rule you actually want to tune is `AllCooldown` in
[Targeting and immunity](targeting.md).

## Data

```lua
StoreName = "",
StudioMode = "Memory",
AutosaveSeconds = 180,
LockStaleSeconds = 600,
LoadAttempts = 5,
```

Player data here is small: purchase credits and the recent purchase ids used for duplicate
protection.

| Field | Meaning |
|---|---|
| `StoreName` | The DataStore name. Empty derives `uxrTrollSystem_Profile_v1` |
| `StudioMode` | What happens when you press Play in Studio |
| `AutosaveSeconds` | How often to save. Each save refreshes the session lock |
| `LockStaleSeconds` | How long a lock stays valid after its server dies |
| `LoadAttempts` | How many times to try acquiring the lock |

### Studio modes

| Mode | Behaviour |
|---|---|
| `"Memory"` | Nothing is read or written. Every test starts fresh |
| `"Isolated"` | A separate DataStore named `<StoreName>_Studio` |
| `"Live"` | The real DataStore |

`"Memory"` is what you want while building, and it also means you do not need Studio access
to API services enabled.

`"Isolated"` is for checking that saving and loading really work without touching a byte of
live data. `"Live"` is for deliberately debugging one player's saved data, and nothing else.

None of the three has any effect on a published game.

### Session locking

A profile is locked by the server holding it, and the lock is refreshed on every autosave.
If that server crashes the lock is left behind, and after `LockStaleSeconds` another server
may take it over.

Keep `LockStaleSeconds` comfortably above `AutosaveSeconds`. The shipped 600 against 180
gives three chances to refresh before a lock is considered stale.

<Callout type="warning" title="Do not change StoreName after going live">

You lose access to every existing profile, which here means every banked credit and the
duplicate-purchase history.

</Callout>

## The folder layout

| Location | Contains |
|---|---|
| `ServerScriptService/uxrTrollSystem` | The package. The only design-time copy |
| `ReplicatedStorage/uxrTrollSystem` | The `Shared` half of each folder, plus `Remotes` and `Assets` |
| `ServerStorage/uxrTrollSystem` | The server-only assets |
| `StarterGui/uxrTrollSystemGui` | The panel |

The package splits into five top-level folders:

| Folder | Holds |
|---|---|
| `Configuration` | Everything you edit |
| `Core` | The bootstrap, networking, logging and the API |
| `Logic` | The services: trolls, purchases and profiles |
| `Assets` | The effect parts |
| `Interface` | The panel |

Each of `Configuration`, `Core` and `Logic` has a `Shared` half that is replicated and a
`Server` half that is not. That split is the whole security model: if something must stay
secret, it goes in a `Server` folder.

<Callout type="info" title="Services are discovered, not listed">

Every `ModuleScript` directly inside `Logic/Server` is loaded as a service, in alphabetical
order, and given `Init` then `Start`.

Adding a service of your own is a matter of dropping a module in. A service that fails to
load logs an error and the rest carry on.

</Callout>

## Where to start

| You want | Edit |
|---|---|
| Different trolls or damage | `TrollSettings.Actions` |
| Less chaos | `TrollSettings.Limits.AllCooldown` |
| To protect somebody | `ImmunitySettings` |
| Different chat lines | `TrollSettings.Announce` and the per-troll messages |
| To sell other products too | `MarketplaceSettings.ManageProcessReceipt` |
| To react to trolls | [Hooks and the API](hooks.md) |
| To turn everything off | `Settings.Enabled` |
