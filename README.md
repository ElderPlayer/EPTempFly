# EPTempFly - Advanced Temporary Flight Plugin (Version 1.2.1)

EPTempFly is a fully modern, comprehensive, and performant plugin that grants your players temporary flight abilities (TempFly). It is designed to make your players' flight experience safe, fair, and highly customizable.

## 🌟 Why EPTempFly?

* **No Time Wasted:** Flight time automatically pauses when players land on the ground or walk (`general.pause-time-when-on-ground`). Time is only deducted when they are actually flying in the air.
* **Fall Damage Protection:** If a player's flight time runs out mid-air, the plugin ensures they land safely and completely prevents fall damage.
* **Visual Feast (Particles):** Players can choose from 68 different particle trail effects (33 new effects added in 1.2.1) to display while flying. Particles with a cost of "0" are offered to players for free.
* **Time Sharing:** Players can gift or transfer their own flight time to others at any time. This action is also fully supported across different servers via proxy networks (BungeeCord/Velocity).
* **Join Rewards:** You can automatically grant free flight time to players when they join your server for the first time, or on every join.

## 🔗 Fully Supported Plugins (Hooks)

EPTempFly seamlessly integrates with many other plugins to ensure it doesn't break your server's existing systems. These hooks are strictly optional and automatically detected if installed.

**Land, Region, and Island Plugins:**
Flight is exclusively permitted within allowed regions for the following plugins:
* **uxmClaims:** Full compatibility with claim roles.
* **SuperiorSkyblock2:** Island flight privilege verification.
* **WorldGuard:** `FLY` flag support to toggle flight in specific regions.
* **FactionsUUID:** Flight in faction lands. (Can be optionally disabled in wilderness, warzones, and safezones).
* **Other Supported Plugins:** GriefPrevention, Residence, PlotSquared, Lands, HuskClaims, ExcellentClaims, BentoBox, IridiumSkyblock, FabledSkyblock, Towny, GriefDefender.

**Combat and PvP Plugins:**
* **PvPManager:** (v4 API Compatible) Flight is instantly disabled when a player enters combat.
* **CombatLogX:** Flight is disabled during combat tagging.

**Economy Plugins (For Shop):**
* **Vault:** Sell flight time using in-game currency.
* **PlayerPoints:** Sell flight time using credits/points.

## 📊 PlaceholderAPI Variables

Useful placeholders for your menus, scoreboards, or chat formatting:
* `%eptempfly_time%` / `%eptempfly_remaining%` - Shows remaining flight time (e.g., 1h 30m).
* `%eptempfly_time_seconds%` - Shows remaining flight time in exact seconds.
* `%eptempfly_flying%` - Returns whether the player is currently flying (True/False).
* `%eptempfly_locked%` - Returns whether the player's flight is administratively locked.
* `%eptempfly_unlimited%` - Returns whether the player has unlimited flight bypass.
* `%eptempfly_inclaim%` - Returns whether the player is in an allowed claim region.
* `%eptempfly_canfly%` - Returns the player's general ability to fly.

## 💾 Database and Server Network (Proxy)

* **Standalone Servers:** Uses extremely fast, zero-setup SQLite (`data.db`) by default. MySQL can be configured if preferred.
* **BungeeCord / Velocity Support:** If you have a multi-server network, simply drop the `EPTempFly.jar` into your proxy's `plugins` folder. Player flight times will automatically synchronize across all backend servers via the `eptempfly:sync` channel, without requiring a shared MySQL table.

## 🌍 Language Support

The plugin supports 8 different languages out of the box, changeable with a single click in `config.yml`:
* English (`en_EN`), Turkish (`tr_TR`), German (`de_DE`), Russian (`ru_RU`), Arabic (`ar_SA`), Chinese (`zh_CN`), Portuguese (`pt_BR`), Albanian (`sq_AL`).

## 💻 Developer API

Easy integration for your custom plugins:
```java
EPTempFly api = Bukkit.getServicesManager().load(EPTempFly.class);
// Or alternatively: EPTempFlyAPI.get()
```
* **Events:** `TempFlyToggleEvent` (Triggered when flight is enabled or disabled).

## ⌨️ Commands and Permissions

**Basic Commands:**
* `/tempfly` - Toggles flight (`eptempfly.use`).
* `/tempfly time [player]` - Checks remaining flight time.
* `/tempfly give <player> <time>` - Sends your own flight time to another player (`eptempfly.give`).
* `/tempfly shop` - Opens the time shop menu (`eptempfly.shop`).
* `/tempfly particles` - Opens the particle trails menu (`eptempfly.particle`).

**Admin Commands (`eptempfly.admin`):**
* `/tempfly add <player> <time>` - Adds flight time to a player.
* `/tempfly set <player> <time>` - Sets a player's flight time.
* `/tempfly remove <player> <time>` - Removes flight time from a player.
* `/tempfly lock <player> [true/false]` - Locks or unlocks a player's flight.
* `/tempfly reload` - Reloads configuration and language files.

**Extra Permissions:**
* `eptempfly.unlimited` - Grants unlimited flight time.
* `eptempfly.bypass.combat` - Bypasses combat flight restrictions.
* `eptempfly.bypass.world` - Allows flight in disabled/blacklisted worlds.
