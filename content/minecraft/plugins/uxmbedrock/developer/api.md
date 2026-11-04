---
title: API
order: 1101
description: Depending on uxmBedrock, the BedrockApi service and its events.
icon: code
---

The API is in the uxmBedrock jar, package `com.uxplima.bedrock.api`. Add the jar as a `compileOnly` dependency and
`uxmBedrock` to your plugin's dependencies, so it loads first.

```java
BedrockApi.get().ifPresent(api -> {
    if (api.isBedrock(player.getUniqueId())) {
        api.client(player.getUniqueId()).ifPresent(client -> {
            boolean touch = client.touch();
        });
    }
});
```

## BedrockApi

| Method | Answers |
|---|---|
| `static Optional<BedrockApi> get()` | The API from the services manager, while uxmBedrock runs |
| `boolean isBedrock(UUID)` | Whether the player is online on Bedrock |
| `Optional<BedrockClient> client(UUID)` | The player's client: device, input, pocket layout, Bedrock version |
| `boolean menusAsForms(UUID)` | Whether the player chose forms for other plugins' menus |
| `boolean wants(UUID, String feature)` | Whether a feature is on for the player: the server has it on and the player did not switch it off |
| `boolean sendForm(UUID, Form)` | Sends a Floodgate (Cumulus) form; `false` for a Java player |
| `Path packFolder()` | The folder the build is written to |

Every method may be called from any thread.

## BedrockClient

| Field | Type |
|---|---|
| `player` | UUID |
| `name` | String |
| `device` | `ANDROID`, `IOS`, `MACOS`, `FIRE_OS`, `WINDOWS`, `PLAYSTATION`, `SWITCH`, `XBOX`, `VR`, `OTHER`, `UNKNOWN` |
| `input` | `KEYBOARD_MOUSE`, `TOUCH`, `CONTROLLER`, `VR`, `UNKNOWN` |
| `pocketLayout` | boolean |
| `version` | String, empty when unknown |

## Events

Both are called off the server threads.

| Event | When | Gives |
|---|---|---|
| `BedrockPackBuiltEvent` | A build finished | `folder()`, `failure()`, `unknownItems()`, `unchanged()` |
| `BedrockChoiceEvent` | A Bedrock player changed their choices | `player()`; ask the API for the new state |

Plugins built on uxmLib reach the same answers through uxmLib's Bedrock and menu modules and need no dependency on
uxmBedrock.
