---
title: The folder model
order: 3
description: What each folder in a package is for, and which copies are safe to edit.
icon: folder-tree
---

A package has four top-level folders. Which folder a file is in decides where it ends up
at runtime and who is allowed to see it.

```
ServerScriptService/
  uxrExampleSystem/            the package you dropped in
    init.server.luau           the bootstrap
    Shared/                    copied to ReplicatedStorage
      Config/Settings.luau     your settings
      Lib/                     Types, Util
    Server/                    stays here, never replicated
      Bootstrap.luau
      Net.luau
      ApiHandlers.luau
      PlayerLifecycle.luau
      Hooks.luau
      Services/ProfileService.luau
    Storage/                   moved to ServerStorage
      Assets/
    Client/                    ScreenGuis, copied to StarterGui
      uxrExampleSystemGui/
```

| Folder | Ends up in | Visible to the client | Edit it? |
|---|---|---|---|
| `Shared` | `ReplicatedStorage/<package>` | Yes | Yes, here in the package |
| `Server` | Stays in `ServerScriptService` | No | Yes, here in the package |
| `Storage` | `ServerStorage/<package>` | No | Yes, here in the package |
| `Client` | `StarterGui/<package>Gui` | Yes | Yes, here in the package |

## Only the ServerScriptService copy is real

At runtime you will see the same names in four places:

```
ServerScriptService / uxrExampleSystem      the source of truth
ReplicatedStorage   / uxrExampleSystem      a copy of Shared, rebuilt every start
ServerStorage       / uxrExampleSystem      Storage, moved every start
StarterGui          / uxrExampleSystemGui   a copy of the interface
```

<Callout type="danger" title="Edits to the runtime copies are thrown away">

The bootstrap destroys and recreates the `ReplicatedStorage` and `ServerStorage` copies
on every single server start. If you edit settings in `ReplicatedStorage` because that
is where you found them, your change survives until the next Play and then vanishes
without a message.

Always edit inside `ServerScriptService/<package>`.

</Callout>

## Server-only means server-only

Anything under `Server/` is never replicated. That is the reason the reward code, the
hooks file and the profile store live there: a player cannot read them, so they cannot
be used to work out what an unclaimed reward is or to fake a grant.

The rule to carry into your own edits: if a file contains a secret, a payout, or logic a
player would benefit from reading, it belongs under `Server/`.

## Where your settings live

| File | What it holds |
|---|---|
| `Shared/Config/Settings.luau` | Every toggle. Replicated, so treat it as public |
| `Server/Hooks.luau` | Optional callbacks for your own analytics or effects |
| Product-specific server files | Reward ladders, action lists, command tables |

Settings are replicated because the interface reads them too. Do not put an API key or
anything else private in there.

## The interface

`Client/<package>Gui` is a `ScreenGui`. The `CoreScript` folder inside it is code and is
managed by the product; everything else is design and is yours.

The bootstrap copies the `ScreenGui` into `StarterGui` **only if a `ScreenGui` of the
same name is not already there**. That is what lets a designer keep an edited copy in
`StarterGui` without the package overwriting it on every test.

Each product's Interface page lists the exact frames and buttons its code expects to
find. Rename one of those and that part of the panel stops working; add or restyle
anything else freely.
