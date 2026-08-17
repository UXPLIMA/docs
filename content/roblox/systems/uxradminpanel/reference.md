---
title: Command reference
order: 5
description: All 346 commands by category, with aliases and the rank each one needs.
icon: book-open
---

Every command in the system, grouped the way the panel groups them. The rank column is the
minimum rank required; see [Ranks and permissions](ranks.md).

Argument syntax is on each command's card in the panel, and the rules behind it are in
[Running commands](commands.md).

<Callout type="info" title="Adding a command takes two files">

Metadata goes in `Shared/Config/Commands.luau` and the implementation in
`Server/Commands/<Category>.luau`. A command present in one and missing from the other is
the single most common reason a new command does not appear.

Nothing else needs registering: the lookup tables are rebuilt on every load.

</Callout>

## Movement

| Command | Aliases | Rank | Does |
|---|---|---|---|
| `fly` | `fl` | Mod | Toggle flight on the target |
| `unfly` |  | Mod | Force-stop flight mode |
| `noclip` |  | Mod | Toggle no-clip walking |
| `unnoclip` | `clip` | Mod | Force-stop no-clip mode |
| `flytool` |  | Mod | Give a FLY tool |
| `unflytool` |  | Mod | Take the FLY tool |
| `nocliptool` |  | Mod | Give a noclip tool |
| `unnocliptool` |  | Mod | Take the noclip tool |
| `walkspeed` | `ws` | Mod | Set the walk speed |
| `jumpspeed` | `jp`, `jumppower`, `jumpheight`, `power` | Mod | Set the jump height |
| `sit` |  | VIP | Force the player to sit |
| `jump` |  | VIP | Force the player to jump |
| `fov` |  | VIP | Set the camera FOV |
| `weld` |  | Admin | Weld target to whatever's in front of you |
| `setspawn` | `setspawnpoint`, `respawnlocation` | Admin | Place a personal SpawnLocation under the target |
| `apparate` | `warpforward` | Mod | Teleport target forward along its look vector |
| `lockplayer` |  | Admin | Lock all body parts (BasePart.Locked = true) |
| `unlockplayer` |  | Admin | Unlock all body parts |
| `boost` |  | Mod | Speed + jump composite buff |
| `control` |  | HeadAdmin | Take over the target's character body (puppet) |
| `uncontrol` |  | HeadAdmin | Release the current control / puppet |
| `superJump` |  | Mod | Mega jump height (200) |
| `heavyJump` |  | Mod | Heavy-feeling boosted jump (100) |
| `fast` |  | Mod | Quick walkspeed (50) |
| `slow` |  | Mod | Half walkspeed (6) |
| `fly2` |  | Mod | Alternate fly variant |
| `noclip2` |  | Mod | Alternate noclip variant |

## Combat

| Command | Aliases | Rank | Does |
|---|---|---|---|
| `kill` |  | Mod | Kill the player instantly |
| `killall` |  | HeadAdmin | Kill everyone but you |
| `damage` |  | Mod | Subtract HP from the player |
| `heal` |  | Mod | Restore HP to the player |
| `sethealth` | `health`, `hp` | Mod | Set HP to exact value |
| `refresh` | `reload`, `respawn` | Mod | Respawn the player's character |
| `clone` |  | Mod | Make a static character copy |
| `forcefield` | `ff` | Mod | Toggle a visible forcefield |
| `unforcefield` | `unff` | Mod | Force-remove the visible forcefield |
| `god` |  | Mod | Toggle godmode invincibility shield |
| `ungod` |  | Mod | Force-remove the godmode shield |
| `explosion` | `boom`, `explode` | HeadAdmin | Detonate at player location |
| `freeze` |  | Mod | Toggle frozen with ice block |
| `unfreeze` |  | Mod | Force-unfreeze the player |
| `nuke` |  | Owner | Spawn a nuke near you |
| `fling` |  | Mod | Launch player with high velocity |
| `healall` |  | Mod | Restore everyone to full HP |
| `fix` |  | Mod | Reset effects + speed/jump/HP without respawn |
| `removeall` |  | HeadAdmin | Strip an effect class from every player |
| `clear` |  | Admin | Wipe clones/nukes/effects from workspace |
| `maxhealth` | `maxhp` | Mod | Set the target's MaxHealth |

## Cosmetic

| Command | Aliases | Rank | Does |
|---|---|---|---|
| `highlight` |  | Mod | Toggle an outline highlight |
| `unhighlight` |  | Mod | Force-remove the highlight |
| `shirt` |  | Mod | Set shirt by asset ID |
| `pants` |  | Mod | Set pants by asset ID |
| `face` |  | Mod | Set face by asset ID |
| `bighead` |  | Mod | Enlarge the player's head |
| `smallhead` |  | Mod | Shrink the player's head |
| `normalhead` |  | Mod | Reset head to normal size |
| `smoke` |  | Mod | Toggle a smoke effect |
| `unsmoke` |  | Mod | Force-remove the smoke effect |
| `fire` |  | Mod | Toggle a fire effect |
| `unfire` |  | Mod | Force-remove the fire effect |
| `sparkles` |  | Mod | Toggle a sparkles effect |
| `unsparkles` |  | Mod | Force-remove the sparkles effect |
| `spin` |  | Mod | Toggle a head spin effect |
| `unspin` |  | Mod | Force-stop the head spin |
| `size` |  | Mod | Multiply the body scale |
| `height` |  | Mod | Multiply body height only |
| `width` |  | Mod | Multiply body width only |
| `giant` |  | Mod | Scale body parts by 5 |
| `dwarf` |  | Mod | Scale body parts by 0.5 |
| `fat` |  | Mod | Increase body depth and width |
| `thin` |  | Mod | Reduce body depth and width |
| `changename` | `name`, `rename`, `nick`, `nickname` | Mod | Set the overhead display name |
| `hidename` |  | Mod | Toggle the overhead name display |
| `showname` |  | Mod | Force-show the overhead name |
| `invisible` | `inv` | Mod | Toggle character invisibility on/off |
| `visible` | `vis` | Mod | Force-show the character body |
| `hideguis` | `nogui`, `hideui` | Mod | Toggle all UIs except chat |
| `showguis` | `showui` | Mod | Force-show all UI screens |
| `material` |  | Mod | Set the body part material |
| `gold` |  | Mod | Turn the body into gold |
| `neon` |  | Mod | Turn the body into neon |
| `ghost` |  | Mod | Turn the body ghostly transparent |
| `glass` |  | Mod | Turn the body into glass |
| `ice` |  | Mod | Turn the body into ice |
| `sword` |  | Mod | Give a basic combat sword |
| `btools` |  | Mod | Give F3X building tools |
| `fart` |  | VIP | Cosmetic green fart effect |
| `clearhats` |  | Mod | Remove all hat accessories |
| `disco` |  | Admin | Start ambient color flash loop |
| `undisco` |  | Admin | Stop the ambient flash loop |
| `skybox` |  | Admin | Open the skybox picker |
| `grayscale` | `b&w` | Admin | Desaturate the screen |
| `saturate` |  | Admin | Over-saturate the screen |
| `contrast` |  | Admin | High-contrast post effect |
| `inverted` | `invert` | Admin | Invert screen colors |
| `tint` | `tintcolor` | Admin | Tint the screen with a color |
| `unvisuals` | `disablevisuals` | Admin | Clear every Lighting effect we placed |
| `color` | `colour`, `paint`, `setcolor` | Mod | Color every body part |
| `reflectance` | `reflect` | Mod | Set body reflectance 0-1 |
| `transparency` | `opacity` | Mod | Set body transparency 0-1 |
| `headsize` | `headscale` | Mod | Set head scale |
| `hipheight` | `hipscale` | Mod | Set humanoid hip height |
| `bodytype` | `bodyscale`, `bodyTypeScale`, `btScale` | Mod | BodyTypeScale 0=R15to1=slender |
| `depth` | `depthscale` | Mod | BodyDepthScale |
| `proportion` |  | Mod | Body proportion 0-1 |
| `squash` |  | Mod | Flatten the character |
| `glitch` | `glitched` | Mod | Avatar randomized transparency/reflectance loop |
| `unglitch` |  | Mod | Stop the glitch loop |
| `title` |  | Mod | Overhead title that survives respawn |
| `untitle` |  | Mod | Remove the overhead title |
| `ragdoll` |  | Mod | Break joints into BallSocketConstraints |
| `unragdoll` |  | Mod | Respawn the character to undo ragdoll |
| `playerlist` |  | Mod | Toggle the PlayerList CoreGui |
| `backpack` |  | Mod | Toggle the Backpack CoreGui |
| `emotesmenu` | `emotemenu`, `emotes` | Mod | Toggle the Emotes CoreGui |
| `chatwindow` | `chatbox` | Mod | Toggle the Chat CoreGui |
| `healthbar` |  | Mod | Toggle the Health CoreGui |
| `captures` |  | Mod | Toggle the Captures CoreGui |
| `selfview` |  | Mod | Toggle the SelfView CoreGui |
| `resetbutton` |  | Mod | Enable/disable the reset button |
| `talk` | `chat` | Admin | Make a player appear to speak in system chat |
| `bubblechat` | `forcebubblechat` | Admin | Render a chat bubble over a player |
| `korblox` |  | Mod | Give Korblox parts |
| `unkorblox` |  | Mod | Remove Korblox parts |
| `dominus` |  | Mod | Give a Dominus hat |
| `undominus` |  | Mod | Remove the Dominus hat |
| `headless` |  | Mod | Hide the head + face |
| `unheadless` |  | Mod | Restore the head + face |
| `clearaccessory` | `clearaccessories` | Mod | Strip every accessory |
| `accessory` | `addhat`, `sethat` | Mod | Give an accessory by asset id |
| `clearfaces` |  | Mod | Remove every face Decal |
| `character` | `char`, `morph` | Mod | Morph into another user's avatar |
| `uncharacter` | `unmorph` | Mod | Reset the morph (respawn) |
| `bundle` |  | Mod | Apply a Roblox bundle by id |
| `unbundle` |  | Mod | Reset bundle (respawn) |
| `unsize` |  | Mod | Reset all body scales to 1 |
| `undwarf` |  | Mod | Reset from dwarf |
| `ungiant` |  | Mod | Reset from giant |
| `unbodytype` |  | Mod | Reset bodytype scale to 0 |
| `undepth` |  | Mod | Reset body depth to 1 |
| `unsquash` |  | Mod | Reset squash to 1 |
| `unwidth` |  | Mod | Reset width to 1 |
| `unheight` |  | Mod | Reset height to 1 |
| `unfat` |  | Mod | Reset from fat |
| `unthin` |  | Mod | Reset from thin |
| `unbighead` |  | Mod | Reset head to normal |
| `swag` |  | Mod | Burst-apply a hand-picked pool of accessories |
| `shine` |  | Mod | Neon + reflectance shine |
| `head` |  | Mod | Apply a head accessory by asset id |
| `potatoHead` |  | Mod | Replace head with the Potato mesh |
| `giantDwarf` |  | Mod | Random size between 0.5x and 5x |
| `nightVision` | `nv` | Mod | Enable green night-vision overlay on the player |
| `unNightVision` | `unnv` | Mod | Disable night-vision overlay |
| `laserEyes` | `le`, `lazerEyes` | Mod | Beam eyes with optional color |
| `unLaserEyes` | `unle` | Mod | Remove the red laser eyes |
| `emote` |  | Mod | Play an equipped emote by name or asset id |
| `danceAndBundleReset` |  | Mod | Reload the player's character (clears dance/bundle morphs) |
| `chatTag` | `ctag` | Mod | Set a custom chat tag prefix |
| `chatTagColor` | `tagColor` | Mod | Set chat tag color (#hex or named) |
| `chatName` | `cname` | Mod | Override the player's chat display name |
| `chatNameColor` | `nameColor` | Mod | Set chat display name color (#hex or named) |

## Inventory

| Command | Aliases | Rank | Does |
|---|---|---|---|
| `clearinventory` | `clearinv` | Mod | Wipe backpack and equipped tools |
| `viewinventory` |  | Admin | Open the player backpack viewer |
| `viewhats` |  | Admin | Open the player hats viewer |
| `viewtools` |  | Admin | Open the player tools viewer |
| `handto` |  | Admin | Give your equipped tool to a player |
| `speedcoil` | `scoil` | Admin | Give a Speed Coil |
| `gravitycoil` | `gcoil` | Admin | Give a Gravity Coil |
| `tool` |  | Admin | Give a Tool from Storage/Tools by name |
| `give` |  | Admin | Generic tool give (alias of tool) |
| `removetools` | `rtools`, `notools`, `deltools` | Mod | Clear Backpack + equipped + StarterGear tools |
| `startergive` | `sgive` | Admin | Give a Storage tool that persists across respawns |
| `starterremove` | `sremove`, `unstartergive`, `unsgive` | Admin | Remove a tool from the target's StarterGear |
| `startertools` | `starttools`, `stools` | Admin | Give every Storage tool to the target (persistent) |
| `tools` | `toollist` | Mod | List the tools available in Storage/Tools |
| `gear` |  | Admin | Give a marketplace gear by asset id |
| `rocket` |  | Admin | Give a classic Rocket Launcher |

## Teleportation

| Command | Aliases | Rank | Does |
|---|---|---|---|
| `tpall` | `teleportall` | Admin | Bring every other player to one target |
| `teleport` | `tp`, `to` | Admin | Teleport you to the player |
| `bring` |  | Admin | Teleport the player to you |
| `tpplayer` | `tpp` | Admin | Move one player to another |
| `warp` |  | Admin | Teleport you to saved warp |
| `view` |  | Admin | Lock camera onto the player |
| `unview` |  | Admin | Release the locked camera |
| `follow` |  | Admin | Cross-server follow another player |
| `livetrack` | `track` | Admin | Open a live dashboard for the player |
| `unlivetrack` | `untrack` | Admin | Close the live dashboard for a player |
| `unlivetrackall` | `untrackall` | Admin | Close every active live dashboard |
| `rejoin` |  | VIP | Teleport back into this same server |
| `join` |  | Admin | Join another player's server (cross-server) |
| `place` | `forceplace` | HeadAdmin | Teleport player(s) to a specific placeId |

## Moderation

| Command | Aliases | Rank | Does |
|---|---|---|---|
| `kick` |  | Admin | Kick with an optional reason |
| `kickall` |  | HeadAdmin | Kick everyone except you |
| `banall` |  | Owner | Permanently ban every player below you |
| `directBan` | `dBan` | HeadAdmin | Permanently ban by username or userId (offline-safe) |
| `ban` | `permBan`, `pBan` | HeadAdmin | Permanently ban the player |
| `tempban` | `timeban` | HeadAdmin | Ban for a set duration |
| `unban` |  | HeadAdmin | Remove an existing player ban |
| `mute` |  | Admin | Permanently mute the player |
| `tempmute` | `timeout` | Admin | Mute for a set duration |
| `unmute` |  | Admin | Remove the player's mute |
| `muteall` |  | HeadAdmin | Mute every player at once |
| `warn` |  | Mod | Add a warning to the player |
| `unwarn` |  | Mod | Remove the most recent warning |
| `warns` | `warnings` | Mod | Show the player's warning history |
| `note` |  | Mod | Leave an admin note on a player (works offline) |
| `unnote` |  | Mod | Remove a note by index from the player |
| `notes` |  | Mod | Show all admin notes on a player |
| `unmuteall` |  | HeadAdmin | Unmute every player at once |
| `jail` |  | Admin | Local jail cell with timer |
| `sendjail` |  | Admin | Global jail surviving respawn |
| `unjail` |  | Admin | Release from any jail type |
| `checkban` | `isbanned` | Mod | Look up Roblox ban status for a user |
| `checkwarn` | `checkwarnings` | Mod | Show the player's warning history (alias of warns) |
| `checkrank` | `playerrank` | Mod | Show the rank of any player (online or offline) |
| `crash` |  | HeadAdmin | Force-disconnect a player |
| `punish` |  | Mod | Soft strike: warn + 15-min mute |
| `chatHijacker` | `spychat` | HeadAdmin | Silently spy on a player's chat messages |
| `unchathijack` | `unspychat` | HeadAdmin | Stop spying on a player's chat |

## Server operations

| Command | Aliases | Rank | Does |
|---|---|---|---|
| `lock` |  | Admin | Prevent new server joins |
| `unlock` |  | Admin | Allow new server joins |
| `pvpon` |  | Admin | Enable PVP for everyone |
| `pvpoff` |  | Admin | Disable PVP with shields |
| `lockdown` | `serverlock` | HeadAdmin | Kick non-whitelisted players |
| `unlockdown` |  | HeadAdmin | Lift the active lockdown |
| `addlockdown` |  | HeadAdmin | Add to the lockdown whitelist |
| `removelockdown` |  | HeadAdmin | Remove from the lockdown whitelist |
| `closeserver` |  | Owner | Close the entire server |
| `shutdown` |  | Owner | Broadcast and shut down server |
| `gshutdown` | `globalshutdown`, `universalshutdown` | Owner | Shut down every running server |
| `globallockdown` | `glockdown`, `universallock` | Owner | Lock down every running server |
| `globalunlockdown` | `gunlockdown`, `universalunlock` | Owner | Lift lockdown across all servers |
| `migrate` | `updateserver` | HeadAdmin | Reload this server onto the latest place version |
| `gmigrate` | `globalmigrate`, `universalmigrate` | Owner | Reload EVERY server onto the latest place version |
| `clearterrain` |  | HeadAdmin | Clear all terrain |
| `colorterrain` |  | Admin | Tint every terrain material |
| `restartserver` |  | Owner | Restart into a fresh instance |
| `createserver` |  | Owner | Reserve a private server |
| `time` |  | Admin | Set the in-game clock time |
| `sunrisetime` |  | Admin | Set the time to sunrise |
| `noontime` |  | Admin | Set the time to noon |
| `sunsettime` |  | Admin | Set the time to sunset |
| `nighttime` |  | Admin | Set the time to midnight |
| `brightness` |  | Admin | Set Lighting brightness 0-10 |
| `ambience` |  | Admin | Set ambient color (rgb or #hex) |
| `fog` |  | Admin | Set FogStart and FogEnd |
| `gravity` |  | Admin | Set workspace gravity (default 196) |
| `ambient` |  | Admin | Set Lighting.Ambient color |
| `outdoorambient` |  | Admin | Set Lighting.OutdoorAmbient color |
| `colorshift` |  | Admin | Set Lighting ColorShift (top + bottom) |
| `exposure` |  | Admin | Set Lighting exposure compensation (-5..5) |
| `shadows` | `globalshadows` | Admin | Toggle Lighting global shadows |
| `fogstart` |  | Admin | Set Lighting.FogStart |
| `fogend` |  | Admin | Set Lighting.FogEnd |
| `diffusescale` |  | Admin | Set Lighting EnvironmentDiffuseScale 0-1 |
| `specularscale` |  | Admin | Set Lighting EnvironmentSpecularScale 0-1 |
| `latitude` | `geographiclatitude` | Admin | Set Lighting GeographicLatitude (-90..90) |
| `atmosphere` | `airdensity` | Admin | Set Atmosphere density 0-1 (creates one if absent) |
| `winddir` | `winddirection` | Admin | Set GlobalWind direction (keeps current speed) |
| `windspeed` |  | Admin | Set GlobalWind speed (keeps current direction) |
| `watercolor` |  | Admin | Set Terrain water color |
| `waterreflectance` |  | Admin | Set Terrain water reflectance 0-1 |
| `watertransparency` |  | Admin | Set Terrain water transparency 0-1 |
| `watersize` |  | Admin | Set Terrain water wave size 0-1 |
| `waterspeed` |  | Admin | Set Terrain water wave speed |
| `savemap` |  | Owner | Snapshot workspace to storage |
| `loadmap` |  | Owner | Restore the saved snapshot |
| `setteam` | `team`, `joinTeam` | HeadAdmin | Assign player to a team |
| `createteam` |  | HeadAdmin | Create a new player team |
| `removeteam` |  | HeadAdmin | Delete a team by name |
| `teamrespawn` | `tmrs`, `trs`, `teamrs` | HeadAdmin | Respawn every player on a team |
| `clearteams` | `ctm`, `cleartm` | HeadAdmin | Delete every team in the game |
| `editteam` | `eteam` | HeadAdmin | Recolor an existing team |
| `randomizeteams` | `randomteams`, `rteams`, `rteam` | HeadAdmin | Shuffle all players across the existing teams |
| `editdata` |  | HeadAdmin | Open the player data editor (leaderstats + nested values) |
| `setstat` | `changestat`, `change` | HeadAdmin | Set a leaderstat to a value |
| `addstat` | `add` | HeadAdmin | Add to a numeric leaderstat |
| `subtractstat` | `substat`, `substractstat`, `subtract` | HeadAdmin | Subtract from a numeric leaderstat |
| `resetstats` |  | HeadAdmin | Zero every leaderstat on the player |
| `removestat` | `remove` | HeadAdmin | Remove a single leaderstat by name |
| `setwarp` |  | HeadAdmin | Save current position as warp |
| `delwarp` |  | HeadAdmin | Delete a named warp point |
| `warps` | `listwarps`, `warplist` | HeadAdmin | List every active warp point |
| `restoremap` |  | Owner | Restore the workspace from the original snapshot |
| `lockMap` |  | HeadAdmin | Anchor and lock every BasePart in workspace |
| `insert` |  | HeadAdmin | Insert a marketplace asset into workspace |
| `globalPlace` |  | Owner | Cross-server teleport one player to a placeId |
| `globalForcePlace` |  | Owner | Cross-server teleport multiple players by name to a placeId |
| `fogColor` |  | Admin | Set Lighting.FogColor (rgb or #hex) |

## Messaging

| Command | Aliases | Rank | Does |
|---|---|---|---|
| `servermessage` | `message`, `m` | Admin | Compose a server-wide message |
| `globalservermessage` | `announce`, `announcement`, `a`, `globalAnnouncement`, `ga`, `broadcast` | HeadAdmin | Compose a cross-server message |
| `privatemessage` | `pm` | Admin | Compose a direct private message |
| `chatmessage` |  | Admin | Compose a system chat message |
| `notifications` | `notifyhistory`, `nhistory` | NonAdmin | Open your notification history |
| `sellgamepass` |  | Admin | Prompt the player to buy a gamepass |
| `sellproduct` |  | Admin | Prompt the player to buy a developer product |
| `sellasset` |  | Admin | Prompt the player to buy a marketplace asset |
| `countdown` | `timer` | Admin | Broadcast a visible countdown to everyone |
| `unblur` |  | Admin | Clear the player's screen blur |
| `hint` | `h`, `serverHint`, `sh`, `shint` | Admin | Persistent top banner for all players |
| `notice` | `notif` | Admin | Send a quiet notice toast to one player |
| `alert` |  | HeadAdmin | High-priority alert banner to every client |
| `systemMessage` | `sm` | Admin | System-style chat broadcast |
| `countdown2` | `timer2` | Admin | T-minus style countdown banner |
| `globalAlert` |  | Owner | Cross-server alert broadcast |
| `globalVote` | `gv`, `gVote`, `globalPoll`, `gPoll`, `gp` | HeadAdmin | Open the vote launcher with global-mode pre-checked |
| `vote` |  | HeadAdmin | Open the vote launcher menu |
| `ping` |  | Admin | Show the player network ping |
| `radio` |  | Admin | Give a portable BoomBox tool |
| `blur` |  | Admin | Blur the player's screen |

## Utility

| Command | Aliases | Rank | Does |
|---|---|---|---|
| `award` |  | HeadAdmin | Award a badge by id |
| `sound` |  | VIP | Play a sound by asset ID |
| `stopsound` |  | VIP | Stop a specific active sound |
| `pitch` | `playbackSpeed` | VIP | Set playback pitch of all active sounds |
| `volume` | `loudness` | VIP | Set volume of all active sounds |
| `pause` | `pausemusic` | VIP | Pause all active sounds (keeps position) |
| `resume` | `resumemusic` | VIP | Resume paused sounds |
| `stopallsounds` | `stop`, `stopmusic`, `musicoff` | VIP | Stop every active sound effect |
| `whois` | `userinfo` | Mod | Show a player's userId, rank, and account age |
| `playercount` | `plrcount`, `countplayers`, `countplrs` | Mod | Show the current player count |
| `serverage` | `uptime` | Mod | Show how long this server has been running |
| `gameid` |  | Mod | Show the GameId (universe id) |
| `jobid` |  | Mod | Show this server's JobId |
| `placeid` |  | Mod | Show the PlaceId |
| `showfps` | `getfps`, `checkfps`, `playerfps` | Mod | Measure and report a player's FPS |
| `setproperty` | `setprop` | Owner | Set an instance property by path |
| `getproperty` | `getprop` | HeadAdmin | Read an instance property by path |
| `loop` |  | Admin | Repeat a command every `<delay>` seconds |
| `unloop` |  | Admin | Stop the loop for a specific command |
| `unloopall` |  | Admin | Stop every active loop you own |

## Help and navigation

| Command | Aliases | Rank | Does |
|---|---|---|---|
| `roleinfo` | `roles`, `ranks` | NonAdmin | Show ranks and their command access |
| `age` | `accountage` | Mod | Show account age of the target |
| `checkpermissions` | `perms`, `mycommands` | NonAdmin | Open the role-info viewer |
| `commandbar` | `cmdbar` | VIP | Open the command bar |
| `uncommandbar` | `uncmdbar` | VIP | Close the command bar |
| `prefix` |  | NonAdmin | Show the current command prefix |
| `panel` |  | NonAdmin | Open the admin panel |
| `commands` | `cmds` | NonAdmin | Open the Commands page |
| `settings` | `preferences`, `config` | NonAdmin | Open the Settings page |
| `logs` | `auditlogs`, `chatLogs`, `clogs`, `banland`, `banlist`, `commandLogs` | Mod | Open the Logs page |
| `manager` | `players` | Mod | Open the Players page |
| `help` | `?` | NonAdmin | Show command usage or list all |

## Rank

| Command | Aliases | Rank | Does |
|---|---|---|---|
| `rank` | `serverrank`, `temprank`, `temporaryrank` | Owner | Assign a temporary runtime rank |
| `permrank` |  | Owner | Assign a persistent saved rank |
| `unrank` |  | Owner | Clear the runtime rank override |
| `unpermrank` |  | Owner | Clear the persistent rank override |
