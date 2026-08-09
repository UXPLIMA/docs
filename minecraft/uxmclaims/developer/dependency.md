---
title: Adding Dependency
order: 420
---

## Bukkit API Module

For Paper/Spigot plugins, you only need **one dependency**:

| Module     | Artifact     | Description              |
|------------|--------------|--------------------------|
| Bukkit API | `bukkit-api` | Complete API for servers |

This module includes everything you need:

- `ClaimFacade` - All claim operations
- `PlayerFacade` - Player operations
- Domain models and events
- Bukkit converters

---

## Gradle (Kotlin DSL)

```kotlin
repositories {
    maven("https://repo.inovixi.com/repository/maven-public/")
}

dependencies {
    compileOnly("com.uxplima.claim:bukkit-api:VERSION")
}
```

## Gradle (Groovy)

```groovy
repositories {
    maven { url 'https://repo.inovixi.com/repository/maven-public/' }
}

dependencies {
    compileOnly 'com.uxplima.claim:bukkit-api:VERSION'
}
```

## Maven

```xml
<repositories>
    <repository>
        <id>uxplima</id>
        <url>https://repo.inovixi.com/repository/maven-public/</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.uxplima.claim</groupId>
        <artifactId>bukkit-api</artifactId>
        <version>VERSION</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

---

## plugin.yml

Add uxmClaims as a dependency:

```yaml
name: MyPlugin
version: 1.0.0
main: com.example.myplugin.MyPlugin
depend:
  - uxmClaims  # use 'depend' if required, 'softdepend' if optional
```

---

## Getting the API Instance

```java
import com.uxplima.claim.bukkit.api.UxmClaimBukkitAPI;
import com.uxplima.claim.app.facade.ClaimFacade;
import com.uxplima.claim.app.facade.PlayerFacade;

public class MyPlugin extends JavaPlugin {
    
    private ClaimFacade claimFacade;
    private PlayerFacade playerFacade;
    
    @Override
    public void onEnable() {
        // Get the API instance
        UxmClaimBukkitAPI api = UxmClaimBukkitAPI.getInstance();
        
        // Get facades
        this.claimFacade = api.claimFacade();
        this.playerFacade = api.playerFacade();
        
        getLogger().info("uxmClaims integration enabled!");
    }
}
```

---

## UxmClaimBukkitAPI Methods

| Method                    | Returns             | Description                  |
|---------------------------|---------------------|------------------------------|
| `getInstance()`           | `UxmClaimBukkitAPI` | Get the API singleton        |
| `claimFacade()`           | `ClaimFacade`       | Get claim operations facade  |
| `playerFacade()`          | `PlayerFacade`      | Get player operations facade |
| `eventBusPort()`          | `EventBusPort`      | Get event bus for listening  |
| `registerEvent(Object)`   | `void`              | Register an event listener   |
| `unregisterEvent(Object)` | `void`              | Unregister an event listener |

---

## Registering Event Listeners

```java
import com.uxplima.claim.bukkit.api.UxmClaimBukkitAPI;
import com.uxplima.claim.bukkit.api.annotation.ClaimEventHandler;
import com.uxplima.claim.domain.event.ClaimCreateEvent;

public class MyClaimListener {
    
    @ClaimEventHandler
    public void onClaimCreate(ClaimCreateEvent event) {
        // Handle event
    }
}

// In your plugin
UxmClaimBukkitAPI.getInstance().registerEvent(new MyClaimListener());
```

---

## Version Compatibility

| uxmClaims Version | Minecraft Version | Java Version |
|-------------------|-------------------|--------------|
| 3.x               | 1.19.4+           | Java 21+     |

---

## Next Steps

- [📖 Query API](../developer/queries.md) - Reading claim data
- [⚡ Events](../developer/events.md) - Listening to events
