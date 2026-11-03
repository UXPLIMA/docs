---
title: Developer API
order: 163
description: Events, queries, actions, the menu API and the REST add-on.
icon: code
---

The API is a published Maven artifact you compile against. Your plugin can watch what uxmEssentials does,
refuse some of it, read its state, and drive it.

| Page | What it covers |
|---|---|
| [Overview](overview.md) | The front door, and what each part of the API is for |
| [Adding the Dependency](dependency.md) | The repository and coordinate, for Maven and Gradle |
| [Events](events.md) | The events published, and the nine you can cancel |
| [Query API](queries.md) | Reading state: homes, balances, punishments, worlds |
| [Action API](actions.md) | Driving the same use cases the commands drive |
| [Menu API](menu-api.md) | Teaching the engine your own actions, requirements and sources |
| [REST API](rest-api.md) | HTTP and WebSocket access for panels, bots and scripts |
