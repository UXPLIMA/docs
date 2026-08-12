---
title: Security
order: 9220
description: A self-service second factor, a join freeze, op-command protection and a same-IP alt guard.
---

Security hardens accounts against what a login alone cannot stop: a shared computer, a session left open, a
compromised launcher. It adds a second factor a player enrols themselves with `/2fa` or `/pin`, a join freeze
for enrolled players arriving from an unrecognised device, a proof prompt in front of dangerous op commands,
and a same-address alt guard for staff. It is not a login system: there is no `/register` and no password.

Module `security` · enabled by default · `modules/security/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/2fa` (`/twofactor`) | Manage your authenticator second factor. | `uxmessentials.security.2fa` |
| `/clientinfo` | Show the client brand a player reported. | `uxmessentials.security.clientinfo` |
| `/ipalts` | List the accounts that share an IP with a player. | `uxmessentials.security.alts` |
| `/pin` | /pin set, change, remove or lock with your numeric PIN second factor. | `uxmessentials.security.pin` |
| `/security` | /security status, force or reset a player's second factors. | `uxmessentials.security.admin` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.security` | op | Hot-reload / inspect the security module (2FA, op-protection, IP/alt guard). |
| `uxmessentials.security.2fa` | everyone | /2fa: set up, confirm or disable an authenticator second factor on your own account. |
| `uxmessentials.security.admin` | op | /security: inspect and manage another player's second factors. |
| `uxmessentials.security.alts` | op | /ipalts: list the accounts that share an IP with a player. |
| `uxmessentials.security.alts.notify` | op | Receive the staff notice when a join shares an IP with other accounts or reports a flagged client. |
| `uxmessentials.security.bypass` | op | Exempt from the two-factor join verification and op-command re-auth checks. |
| `uxmessentials.security.clientinfo` | op | /clientinfo: show the client brand a player reported. |
| `uxmessentials.security.force` | op | /security force \<player>: force a player to re-verify their second factor on their next action or join. |
| `uxmessentials.security.pin` | everyone | /pin: set, change or remove a numeric PIN second factor on your own account. |
| `uxmessentials.security.pin.required` | off | Must set a PIN before playing. |
| `uxmessentials.security.reset` | op | /security reset \<player> [totp\|pin\|all]: clear a factor a player can no longer prove. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `two-factor.enabled` | `true` | The master switch for two-factor enrolment. With this off, /2fa and /pin refuse to enrol (existing factors can still be removed with /2fa disable). |
| `two-factor.totp` | `true` | Which factors a player may enrol. TOTP is an authenticator-app code; PIN is a short numeric code. Turn either off to offer only the other. |
| `two-factor.pin` | `true` |  |
| `two-factor.issuer` | `"uxmEssentials"` | The issuer label shown next to the account in the authenticator app (the "uxmEssentials (Steve)" heading). Set it to your server's name so a player with several servers can tell the entries apart. |
| `two-factor.code-window` | `1` | How many 30-second time-steps of tolerance to allow when checking a submitted code, on either side of the current step. 1 (±30s) absorbs modest clock drift between the server and the player's phone; 0 is strictest. Capped at 5. |
| `two-factor.pin-min-length` | `6` | The allowed length range for a PIN, in digits. A PIN outside this range is refused at /pin set. A PIN is always digits only, because the verification screen is a numeric keypad. Six is the shipped minimum because a four-digit PIN is only ten thousand guesses; lower it only if you accept that. |
| `two-factor.pin-max-length` | `8` |  |
| `two-factor.blocked-pins` | `[...]` | PINs refused outright, however well-formed they are. Length rules cannot catch these: 1234 is a perfectly valid four-digit PIN and also the first thing anybody guessing would try. Add your own (your server's founding year, its name spelled on a phone keypad) or set this to [] to refuse none. |
| `join-verification.enabled` | `true` | The master switch for the join freeze. With this off, enrolment still works (/2fa, /pin) but nobody is frozen on join; useful if you want the second factor only for op-command protection in a later release. |
| `join-verification.trust-devices` | `true` | Whether a successful verification remembers the device so the next join from the same address skips the keypad until the trust lapses. The address is stored only as a one-way hash, never in the clear. Turn this off to make every join re-verify. |
| `join-verification.trust-duration-hours` | `24` | How long a remembered device skips the keypad, in hours. After this, the player verifies again. 0 disables the trust window entirely (equivalent to trust-devices = false). |
| `join-verification.max-attempts` | `3` | How many wrong PINs/codes a player may enter before they are kicked. At least 1. |
| `join-verification.lockout-seconds` | `300` | How long, in seconds, a kicked-out player is locked out; a rejoin within this window is kicked immediately. |
| `join-verification.lockout-bans` | `true` | Whether that lockout is also a real tempban on the server's own ban list, rather than only an in-memory cooldown a restart would clear. It goes through the moderation module's ban system, so it lands in the same ban table and punishment history as every other ban, staff see it, and /unban lifts it: this module keeps no ban list of its own. With the moderation module disabled there is nothing to write to and the lockout stays in memory. |
| `join-verification.lockout-ban-reason` | `"Too many failed verification attempts"` | The reason recorded against that ban, shown to the player and in the punishment history. |
| `join-verification.entry-timeout-seconds` | `60` | How long, in seconds, a frozen player has to prove their factor before they are kicked. Without a limit a player who walks away mid-prompt sits at the keypad forever, holding a slot on a full server and leaving their account logged in. 0 removes the limit. |
| `join-verification.spectator-mode` | `adventure` | What to do with a player who has to verify while they are in spectator mode. A spectator cannot click any window the server opens for them, so a spectator shown the keypad can see every button and press none of them: they are stuck. This moves them into a mode that can click for the length of the freeze and puts their own mode back the moment they verify. adventure (they cannot edit the world) \| survival \| none (leave them stuck; know what you are choosing). |
| `join-verification.safety-net` | `kick` | What happens to a joining player when the verification decision itself fails: the database is unreachable, the key-file is unreadable, or the lookup throws. Someone has to decide, because the freeze goes on before the decision runs. kick refuses the join with a "try again" message, so an outage keeps enrolled accounts shut rather than open. allow lifts the freeze and lets them in unverified, so an outage keeps the server playable and accepts that for its duration an enrolled account is protected by its password alone. |
| `join-verification.restrictions.move` | `true` |  |
| `join-verification.restrictions.commands` | `true` |  |
| `join-verification.restrictions.chat` | `true` |  |
| `join-verification.restrictions.interact` | `true` |  |
| `join-verification.restrictions.drop` | `true` |  |
| `join-verification.restrictions.pickup` | `true` |  |
| `join-verification.restrictions.block-edit` | `true` |  |
| `join-verification.restrictions.damage-taken` | `true` |  |
| `join-verification.restrictions.damage-dealt` | `true` |  |
| `join-verification.restrictions.mob-targeting` | `true` |  |
| `join-verification.restrictions.teleport` | `true` |  |
| `join-verification.restrictions.inventory` | `true` |  |
| `join-verification.restrictions.consume` | `true` |  |
| `join-verification.restrictions.hunger` | `true` |  |
| `join-verification.holding-area` | `""` | Where a verifying player is held for the length of the freeze, as world,x,y,z or world,x,y,z,yaw,pitch. The freeze already stops them acting, but they are still standing wherever they logged out, in view of whoever walks past. Naming a spot here puts the unverified session somewhere deliberately empty and puts them back exactly where they were the moment they verify. Blank (the default) leaves them where they are. |
| `join-verification.transfer-to` | `""` | A proxy backend to send the player to once they verify, by its name in the proxy config. This is for the network layout where players land on a small verification server first and are moved to the real one afterwards. Blank (the default) leaves them on this server. Needs a proxy in front; with none there is nothing to send them to and the transfer is a logged no-op. |
| `join-verification.on-access-revoked` | `reverify` | What happens to a player who is online at the moment staff clear their second factor with /security reset. That command is the recovery door, for someone who lost their authenticator or forgot their PIN; it is also, from the other side, exactly what an account theft would want, so what it does to the session it lands in is your call. reverify; send them back through verification at once. With a factor left they prove it again; with none left and a required PIN they are shown the create pad, so a reset ends with a factor rather than without. kick: disconnect them, so nothing of the revoked session carries on and their next join decides afresh. nothing: leave them playing. Right when a reset is routine help-desk work. |
| `join-verification.wait-for-login-plugin` | `true` | Whether to let an installed login plugin (AuthMe and its forks) finish authenticating the player before this module asks for a second factor. On an offline-mode server this ordering is the whole point: until the login plugin says so, the name is only what the client typed, and asking that stranger for the account holder's PIN is both wrong and useless. With no login plugin installed this changes nothing. |
| `feedback.titles` | `true` | Whether the keypad title and the two outcome titles are shown at all. |
| `feedback.sounds.prompt` | `"block.note_block.pling"` |  |
| `feedback.sounds.key` | `"ui.button.click"` |  |
| `feedback.sounds.success` | `"entity.player.levelup"` |  |
| `feedback.sounds.failure` | `"entity.villager.no"` |  |
| `op-protection.enabled` | `true` | The master switch for op-command protection. With this off, protected commands run without any re-auth check. |
| `op-protection.reauth-window-seconds` | `60` | How long, in seconds, a successful verification (a join proof or an earlier re-auth) counts as recent, so a burst of protected commands only prompts once. 0 forces a fresh proof for every protected command. |
| `op-protection.protected-commands` | `[...]` | The commands to protect, by root name. Matching ignores a leading slash, any arguments, and a namespace prefix, so "op" also catches "/op Steve" and "minecraft:op". Add your server's own dangerous commands here. |
| `ip-guard.enabled` | `true` | The master switch for the IP/alt guard: the account cap and the staff notice. Turning it off stops both. The join history behind /ipalts is shared with moderation's /alts, so it keeps being recorded either way; to record nothing at all, disable both the security and the moderation modules. |
| `ip-guard.max-accounts-per-ip` | `0` | The greatest number of DISTINCT accounts allowed to connect from one address. A join that would push an address past this is kicked. 0 (the default) means no cap; the guard only observes and notifies, never kicks. Set it to, say, 3 to hold each household to three accounts. |
| `ip-guard.notify-staff` | `true` | Whether to notify online staff (holders of uxmessentials.security.alts.notify) when a joining player shares an address with other accounts. The notice carries the account names and a count; never the address itself. |
| `client-id.enabled` | `true` | The master switch for the client-brand guard. With this off, no brand is read and /clientinfo has nothing to show. |
| `client-id.mode` | `flag` | How the brand list below is applied: block-list: deny the listed brands, allow everything else (keep known cheat clients out) allow-list: allow ONLY the listed brands, deny everything else (admit a fixed set of approved clients) flag: never deny; a listed brand is only flagged to staff and logged (observe before enforcing) An unrecognised value falls back to flag, the safe mode that never kicks. |
| `client-id.brands` | `[]` | The brands the mode above acts on, matched case-insensitively. Empty by default, so flag mode simply records every brand for /clientinfo without flagging anything. |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_security_enforced%` | Whether the server asks players to verify on join at all (yes/no). |
| `%uxmessentials_security_verifying%` | Whether the player has an open verification challenge they have not answered (yes/no). |
{/* /generated */}

## Notes

- **The module is enabled but inert.** Nothing changes for anyone until a player enrols, or until you make a PIN
  mandatory.
- **The join freeze is a keypad, not a chat prompt.** An enrolled player joining from an unrecognised device
  taps their PIN into a chest window, or switches to a text prompt for an authenticator code. A player who has
  enrolled nothing is never frozen.
- **`uxmessentials.security.pin.required` makes a PIN compulsory** for whoever holds it. A player who holds it
  with no factor set is shown a create pad instead, asking for the PIN twice, since a frozen player cannot run a
  command to comply.
- **On an offline-mode server the login plugin goes first.** With `wait-for-login-plugin` on, the freeze stands
  down until AuthMe or nLogin reports the player authenticated, because until then the name is only what the
  client typed. Detection needs no configuration.
- **The alt guard stores a token, not an address.** The address is turned into a one-way HMAC keyed by the
  server's own secret; nothing is reversible and there is no GeoIP. The raw address is kept beside it only while
  the moderation module is on, which is what `/seenip` and a strict IP ban need.
- **Op-command protection asks for a fresh proof** before the listed commands run, so a session left open on an
  unlocked computer cannot be used to `/op` anybody.

Related: [Moderation](moderation.md), [Staff](staff.md), [Discord Link](discordlink.md)
