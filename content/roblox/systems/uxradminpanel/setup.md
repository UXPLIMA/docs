---
title: Setup
order: 1
description: Install, take ownership, set the shared secret, and add the assets commands need.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrAdminPanelSystem` into
**ServerScriptService** and press Play once.

## 1. Take ownership

`Shared/Config/Permissions.luau` ships with a populated Owner assignment.

```lua
Owner = {
    Players    = { 3057647029 },
    Groups     = { {33106532, 255} },
},
Admin = {
    Groups     = { {33106532, 2} },
},
VIP = {
    Groups     = { {33106532, 1} },
},
```

<Callout type="danger" title="Those ids are not yours">

The user id and the group id in the shipped file belong to somebody else. Left as they are,
a stranger holds Owner in your game, and members of a group you have nothing to do with hold
Admin and VIP.

Replace all four before your first publish. Empty every list you do not need:
`Players = {}`, `Groups = {}`.

</Callout>

`AutoRankOwner = true` makes the game creator an Owner without any list, which is usually
all you need to get started:

```lua
Permissions.AutoRankOwner = true
```

## 2. Set the shared secret

`Server/SharedSecret.luau`:

```lua
return {
    value = "REPLACE_WITH_YOUR_OWN_GUID_BEFORE_SHIPPING",
}
```

Every cross-server message carries this value, and messages arriving without a match are
dropped with a warning. Generate a GUID and paste it in.

<Callout type="warning" title="It has to be the same everywhere and different from anybody else's">

Every server of your game reads the same file, so they agree automatically. What matters is
that you do not ship the placeholder: cross-server messages travel over your own topics, and
a shared placeholder is a shared namespace.

Change it and cross-server announcements, votes, lockdowns and shutdowns stop being
something a copy of this system elsewhere could hear.

</Callout>

## 3. Add the assets commands need

Several commands look for things you supply, in `Storage`:

| Folder | Used by |
|---|---|
| `Tools` | `tool`, `give`, `startergive`, `startertools`, `tools` |
| `Builds` | `jail`, which clones `Builds.JailCell` |
| `Hats` | The hat viewer and accessory commands |
| `Skybox` | The skybox picker |
| `MapSave` | `savemap` and `loadmap` |

All five ship empty. A command whose asset is missing tells the admin exactly what is
missing rather than failing silently: the jail command, for example, reports that
`Builds.JailCell` is absent from `Storage/Builds`.

### The jail cells

| Command | Needs |
|---|---|
| `jail` | A model named `JailCell` in `Storage/Builds`, containing a `SpawnPart` |
| `sendjail` | A model named `PublicCell` in `Workspace/uxrAdminPanelSystem/Builds`, containing a `SpawnPart` |

`jail` clones the template per player and names the clone after them. `sendjail` uses a
single shared public cell that you place yourself.

## 4. Point the webhook somewhere, or leave it

`Server/Config/Webhooks.luau`:

```lua
CommandWebhook = {
    ["*"] = "YOUR WEBHOOK LINK HERE",
    -- ["ban"]  = "a different webhook",
},
```

The placeholder string is recognised and skipped, so leaving it alone simply means no
webhooks. See [Logs, analytics and webhooks](logs.md).

<Callout type="info" title="This file is server-only">

`Server/Config` never leaves the server, which is why the webhook URL lives there and not in
`Shared/Config`. A webhook URL is a write credential for your Discord channel; anybody who
has it can post to it.

</Callout>

## 5. Check it works

| Step | Expected |
|---|---|
| Join as the creator | Two welcome toasts naming your rank |
| Press `/` | The panel opens |
| Press `;` | The command bar opens |
| Type `u!fly me` in chat | You fly |
| Open the Logs page | The command you just ran is there |

Every command is also typeable in chat with the prefix, because the system registers a
`TextChatCommand` for each command and each alias at startup.

## 6. Turn the noise down

```lua
Debug = true,
WelcomeNotification = true,
```

`Debug` prints routine activity to Output; turn it off before you publish. Warnings and
errors are shown either way.

`WelcomeNotification = false` hides the two join toasts for everybody, admins included.
Some games want the panel to be invisible to players who cannot use it, and the floor rank
gives everybody a rank, so everybody sees a toast unless you turn it off.

## What you actually configure

| File | Holds |
|---|---|
| `Shared/Config/Permissions.luau` | Ranks, who holds them, and page gating |
| `Shared/Config/Settings.luau` | Prefix, rate limits, warning policy, messages |
| `Shared/Config/Commands.luau` | Every command's rank, arguments and logging |
| `Server/Config/Webhooks.luau` | Discord webhook URLs |
| `Server/SharedSecret.luau` | The cross-server secret |

Everything else is engine code.
