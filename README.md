# EPTempFly Wiki

**Author:** ElderPlayer  
**Discord:** @elderplayerr  
**Version:** 1.2.1

EPTempFly gives players temporary flight. Flight time can be given by staff, shared between players, bought in a shop, or granted as a join reward. While a player is flying above the ground, an optional particle trail is shown.

Claim and island flight rules are unchanged. Optional systems turn themselves off when a supporting plugin is missing.

---

## Requirements

- **Java 21** or newer (plugin disables itself on older Java)
- Spigot / Paper / Purpur (API 1.17 → current)
- No hard dependencies

---

## Install

1. Put `EPTempFly-1.2.1.jar` in the backend `plugins` folder.
2. Restart. Libraries (SQLite / MySQL) are downloaded from `plugin.yml`.
3. Edit `plugins/EPTempFly/config.yml` as needed.
4. `/tempfly reload`

### Proxy sync (BungeeCord / Velocity)

Put the **same jar** into the proxy `plugins` folder.

- BungeeCord → `bungee.yml` (`com.eptempfly.proxy.BungeeBridge`)
- Velocity → `velocity-plugin.json` (`com.eptempfly.proxy.VelocityBridge`)
- Channel: `eptempfly:sync`

Do not use MySQL shared table **and** proxy sync together. Pick one.

---

## What’s new in 1.2.1

### Flight paused action bar (bugfix)
When a player lands and fly time is paused (`general.pause-time-when-on-ground`), the **Flight paused / Flytime** action bar is shown **once** and then clears. It no longer spam every second while on the ground.

### +33 particle trails
Simple trails added to `particles.yml` (total ~68 enabled options), including:

`dripwater`, `driplava`, `snowball`, `slime`, `waterwake`, `waterdrop`, `mobspell`, `ambient`, `instant`, `spell`, `explosion`, `largeexplosion`, `smoke_large`, `spit`, `sneeze`, `composter`, `flash`, `falling_nectar`, `falling_honey`, `falling_lava`, `falling_water`, `landing_lava`, `dripping_honey`, `dripping_obsidian`, `falling_obsidian`, `landing_obsidian`, `electric_spark`, `cherry`, `white_ash`, `small_flame`, `soot`, `itemcrack`, `blockcrack`, `blockdust`

Edit names, costs and materials in `particles.yml`. New defaults appear after `/tempfly reload` (existing keys are not overwritten if you already customized the file).

### PvPManager support
Already integrated via reflection:

- Class: `me.chancesd.pvpmanager.player.CombatPlayer`
- `CombatPlayer.get(Player)` + `isInCombat()`
- Event: `me.chancesd.pvpmanager.event.PlayerTagEvent`

Config:

```yaml
combat:
  hook-external: true
  pvpmanager: true
pvp:
  enabled: true   # must be true for combat hooks to stop flight
```

Matches the [official Developer API](https://github.com/ChanceSD/PvPManager/wiki/Developer-API). Compatible with PvPManager v4 `CombatPlayer` API.

### Rate limit (planned / partial)
Language key `rate-limited` is present in all lang files for a 3-action → 3s cooldown on menus/commands. Full enforcement requires a source rebuild that wires `RateLimiter` into `MenuListener` and `EPTempFlyCommand` (not in this bytecode-only 1.2.1 patch). Message is ready for the next full source release.

### Performance notes (from Spark)
Spark showed cost in:

| Hot path | Mitigation in 1.2.1 / recommendations |
|---|---|
| `FlyManager.tick` → pause action bar every second | **Fixed:** notify only once via `pauseNotified` |
| `Messages.sendActionBarRaw` (Gson / component parse) | Fewer action bar sends while paused; keep countdown only while actually flying |
| `UxmClaimsHook.findClaim` (reflection) | Claim lookup is already Method-cached; prefer chunk APIs when available |
| `TrailManager.play` resolve every tick | Cache particle enum per trail id after first resolve (source-level improvement) |

Tips:

- Disable unused claim hooks in `hooks.yml`
- Turn off particles if not needed: `particles.enabled: false`
- Prefer `display.countdown.actionbar: false` on very large player counts if action bar still shows in profiles

---

## Features

- Toggle flight with `/tempfly` when the player has time left
- Time pauses on the ground (`general.pause-time-when-on-ground`)
- Fall damage protection after flight ends
- Action bar countdown (and one-shot pause notice)
- Staff add / set / remove / lock
- Players give their own time to someone else (including proxy)
- Shop GUI (Vault or PlayerPoints)
- Trail GUI; particles while flying in the air
- Join reward (one-time or repeating)
- Claim / island / region hooks
- FactionsUUID, WorldGuard flag, PlaceholderAPI
- CombatLogX + **PvPManager** (when `pvp.enabled` is true)
- SQLite or MySQL

---

## Commands

| Command | Description | Permission |
|---|---|---|
| `/tempfly` | Toggle flight | `eptempfly.use` |
| `/tempfly time [player]` | Remaining time | use / admin for others |
| `/tempfly give <player> <time>` | Give your own time | `eptempfly.give` |
| `/tempfly shop` | Fly time shop | `eptempfly.shop` |
| `/tempfly particles` | Trail menu | `eptempfly.particle` |
| `/tempfly add <player> <time>` | Add time | `eptempfly.admin` |
| `/tempfly set <player> <time>` | Set time | `eptempfly.admin` |
| `/tempfly remove <player> <time>` | Remove time | `eptempfly.admin` |
| `/tempfly lock <player> [true/false]` | Lock flight | `eptempfly.admin` |
| `/tempfly reload` | Reload config and language | `eptempfly.admin` |

Aliases: `tfly`, `eptempfly`, `particle`, `trails`.

Time examples: `90`, `10m`, `1h30m`, `1d2h`.

---

## Permissions

- `eptempfly.use` (default: true)
- `eptempfly.shop` (default: true)
- `eptempfly.particle` (default: true)
- `eptempfly.give` (default: true)
- `eptempfly.admin` (default: op)
- `eptempfly.unlimited` — unlimited flight time
- `eptempfly.bypass.combat` — ignore combat flight disable
- `eptempfly.bypass.world` — ignore world disable list

---

## Config highlights

```yaml
general:
  language: en_EN
  pause-time-when-on-ground: true
  auto-enable-on-claim-enter: true
  disable-on-claim-exit: true
  require-claim: true
  fall-protection-ticks: 40

display:
  countdown:
    actionbar: true
  actionbar-paused:
    actionbar: true   # shown once when landing
    chat: false

pvp:
  enabled: false
  cooldown-seconds: 10
  auto-reenable: false

combat:
  hook-external: true
  pvpmanager: true

particles:
  enabled: true
```

---

## PlaceholderAPI

When PlaceholderAPI is present:

- `%eptempfly_time%` / remaining time
- `%eptempfly_flying%` — true/false
- Other keys depend on the installed expansion registration

---

## Developer API

```java
EPTempFly api = Bukkit.getServicesManager().load(EPTempFly.class);
// or EPTempFlyAPI.get()
```

Events: `TempFlyToggleEvent` (ENABLE / DISABLE).

---

## Support hooks (It works without relying on any plugins; it only provides support.)

uxmClaims, GriefPrevention, Residence, PlotSquared, Lands, HuskClaims, ExcellentClaims, SuperiorSkyblock2, BentoBox, IridiumSkyblock, FabledSkyblock, Towny, GriefDefender, WorldGuard, FactionsUUID, PlaceholderAPI, Vault, PlayerPoints, CombatLogX, **PvPManager**

---

## Changelog 1.2.1

- Fix: paused flight action bar no longer repeats every second on the ground
- Add: 33 additional simple particle trails
- Confirm: PvPManager CombatPlayer API hook
- Lang: `rate-limited` key prepared in all languages
- Version bump to 1.2.1
