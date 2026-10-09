# EPTempFly Wiki

Author: ElderPlayer  
Discord: @elderplayerr  
Version: 1.1.0

EPTempFly gives players temporary flight. Flight time can be given by staff, shared between players, bought in a shop, or granted as a join reward. While a player is flying above the ground, an optional particle trail is shown.

The existing claim and island flight rules are unchanged. New systems are optional and turn themselves off when a supporting plugin is missing.

## Requirements

- Java 21 or newer. The plugin disables itself on older Java.
- Spigot, Paper, Purpur or another Spigot fork. API level is 1.17, so 1.17 through current Paper/Spigot builds are supported when the server itself runs on Java 21.
- No hard dependencies.

Servers older than 1.17, and servers that cannot run Java 21, are outside the supported range. One jar cannot run on both modern Java 21 and legacy Java 8.

## Install

1. Put `EPTempFly-1.1.0.jar` in the backend `plugins` folder.
2. Restart. Paper/Spigot downloads the SQLite and MySQL libraries from `plugin.yml`.
3. Edit `plugins/EPTempFly/config.yml` if you want the shop, trails, rewards or extra hooks.
4. `/tempfly reload`

### Proxy sync (BungeeCord and Velocity)

Put the **same jar** into the proxy `plugins` folder. No proxy config file is required.

- BungeeCord loads `bungee.yml` (`com.eptempfly.proxy.BungeeBridge`).
- Velocity loads `velocity-plugin.json` (`com.eptempfly.proxy.VelocityBridge`).
- Backends use the channel `eptempfly:sync`.

On join, a backend asks the proxy for that player's time. If the proxy has never seen the player, it keeps the backend database value and stores it. Later servers receive that value. Giving time with `/tempfly give` works across servers when the target is online on the network, or was seen by the proxy before.

If the proxy plugin is not installed, `proxy.enabled` does nothing harmful: the server keeps using SQLite or MySQL as before.

Do not point two backends at one MySQL table **and** enable proxy sync. Pick one shared source. Proxy sync is the one that matches a network where each backend has its own local database.

## Features

- Toggle flight with `/tempfly` when the player has time left
- Time pauses on the ground (`general.pause-time-when-on-ground`)
- Fall damage protection after flight ends
- Action bar countdown
- Staff add / set / remove / lock
- Players give their own time to someone else, including across the proxy
- Shop GUI for fly time (Vault or PlayerPoints)
- Trail GUI; particles spawn while flying in the air
- One-time or repeating join reward
- Claim, island and region hooks (unchanged behaviour)
- FactionsUUID regions
- WorldGuard flag
- PlaceholderAPI
- CombatLogX, only while `pvp.enabled` is true
- SQLite or MySQL

## Commands

| Command | Description | Permission |
|---|---|---|
| `/tempfly` | Toggle flight | `eptempfly.use` |
| `/tempfly time [player]` | Remaining time | use / admin for other players |
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

## Permissions

- `eptempfly.use` (default: true)
- `eptempfly.shop` (default: true)
- `eptempfly.give` (default: true)
- `eptempfly.particle` (default: true)
- `eptempfly.unlimited` (default: false)
- `eptempfly.admin` (default: op)
- `eptempfly.bypass.combat` (default: op)
- `eptempfly.bypass-join-cleanup` (default: op)

A trail can also set its own `permission` in the config. An empty permission means it is not extra-gated.

## Placeholders

PlaceholderAPI is a soft depend. If it is missing, the expansion is skipped.

- `%eptempfly_time%` / `%eptempfly_remaining%`
- `%eptempfly_time_seconds%`
- `%eptempfly_flying%`
- `%eptempfly_locked%`
- `%eptempfly_unlimited%`
- `%eptempfly_inclaim%`
- `%eptempfly_canfly%`

## Shop and economy

`shop.currency` is `auto`, `vault` or `playerpoints`.

- Vault is used when an economy provider is registered.
- Set `shop.prefer-playerpoints: true` to charge PlayerPoints instead while still having Vault installed.
- If neither plugin is installed, free offers (`cost: 0`) still work. Paid clicks are refused and the server keeps running.

### Particles
Simple trails added to `particles.yml` (total ~68 enabled options), including:

`dripwater`, `driplava`, `snowball`, `slime`, `waterwake`, `waterdrop`, `mobspell`, `ambient`, `instant`, `spell`, `explosion`, `largeexplosion`, `smoke_large`, `spit`, `sneeze`, `composter`, `flash`, `falling_nectar`, `falling_honey`, `falling_lava`, `falling_water`, `landing_lava`, `dripping_honey`, `dripping_obsidian`, `falling_obsidian`, `landing_obsidian`, `electric_spark`, `cherry`, `white_ash`, `small_flame`, `soot`, `itemcrack`, `blockcrack`, `blockdust`

Edit names, costs and materials in `particles.yml`. New defaults appear after `/tempfly reload` (existing keys are not overwritten if you already customized the file).

## Join reward

```yaml
rewards:
  enabled: false
  once: true
  time: 10m
```

Claimed players are stored in `rewards.yml` when `once` is true.

## Hooks

Every hook is optional. A missing plugin is logged and skipped. A hook that throws is caught and does not take the server down.

| Hook | Config | Role |
|---|---|---|
| uxmClaims | `hooks.uxmclaims` | Claim roles, including a renamed default role |
| GriefPrevention | `hooks.griefprevention` | Claim flight |
| Residence | `hooks.residence` | Residence flight |
| PlotSquared | `hooks.plotsquared` | Plot flight |
| Lands | `hooks.lands` | Lands flight |
| HuskClaims | `hooks.huskclaims` | Claim flight |
| ExcellentClaims | `hooks.excellentclaims` | Claim flight |
| SuperiorSkyblock2 | `hooks.superiorskyblock` | Island fly privilege |
| BentoBox | `hooks.bentobox` | Island rank |
| IridiumSkyblock | `hooks.iridiumskyblock` | Island flight |
| FabledSkyblock | `hooks.fabledskyblock` | Island flight |
| Towny | `hooks.towny` | Town flight |
| GriefDefender | `hooks.griefdefender` | Claim flight |
| WorldGuard | `hooks.worldguard` | Region flag (`flag-name`, default `FLY`) |
| FactionsUUID | `hooks.factionsuuid` | Faction land. Not wilderness, safezone or warzone unless configured |
| Vault | shop | Economy |
| PlayerPoints | shop | Points currency |
| PlaceholderAPI | `placeholderapi.enabled` | Placeholders |
| CombatLogX | `combat.hook-external` | Extra in-combat check, only if `pvp.enabled` is true |

FactionsUUID options:

```yaml
hooks:
  factionsuuid:
    enabled: false
    allow-wilderness: false
    members-can-fly: true
    owners-only: false
```

The hook looks for the plugins `Factions` or `FactionsUUID` and only uses the API when those classes exist.

## Storage

```yaml
storage:
  method: sqlite   # or mysql
```

SQLite file: `plugins/EPTempFly/data/data.db`.

MySQL keys live under `mysql` (`username`, `password`, `hostname`, `database`, `tablePrefix`). The comment in the config still applies: do not share one table between servers unless you know they must share it. Proxy sync is the supported way to share time.

### Proxy sync (BungeeCord / Velocity)

Put the **same jar** into the proxy `plugins` folder.

- BungeeCord → `bungee.yml` (`com.eptempfly.proxy.BungeeBridge`)
- Velocity → `velocity-plugin.json` (`com.eptempfly.proxy.VelocityBridge`)
- Channel: `eptempfly:sync`

Do not use MySQL shared table **and** proxy sync together. Pick one.

## Languages

`general.language`: `en_EN`, `tr_TR`, `zh_CN`, `ru_RU`, `ar_SA`, `de_DE`, `pt_BR`, `sq_AL`.

Language files at `lang-version` 4 are replaced on startup with version 5. A backup is written next to the file first.

## What did not change

Claim checks, owner-only mode, auto enable on enter, disable on exit, pause while standing, PvP damage cooldown, teleport re-check, world blacklist and the SQL time format behave as in 1.0.4. New menus and the proxy channel sit beside that loop.
