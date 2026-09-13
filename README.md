# RPGMaker Core

A data-driven RPG framework for Minecraft Paper 1.21+. Build content through Packs and add-ons —
no new plugin, no recompiling.

Think of it as RPG Maker for a Minecraft server: 72 modules covering entities, combat, quests,
dialogue, economy, guilds, parties, arenas, dungeons, seasons, pets, mounts, cinematics and more,
all driven from YAML.

## Install

Drop `RPGMaker-0.1.0.jar` into `plugins/` and restart. Paper 1.21.x, Java 21.

```
plugins/RPGMakerCore/
├── config.yml        # database, flush cadence, tick period
└── packs/            # your content goes here
```

Add-on authors compile against `RPGMaker-API-0.1.0.jar` (interfaces only).

## Your first pack

```
packs/myserver/
├── pack.yml
├── entity/flying_merchant.yml
├── dialogue/merchant_intro.yml
├── loot_table/merchant_drops.yml
└── config/pvp.yml
```

`pack.yml`:

```yaml
name: myserver
namespace: ms
version: 1.0.0
api-version: 1.0
```

A new entity with dialogue, combat and loot is one YAML file plus a component mix — no Kotlin.
See `docs/10-acceptance-test.md` in the source repo for a worked example.

## Admin console

```
/rgm help              # every command, grouped by module
/rgm gui               # the admin panel
/rgm registry types    # what content is loaded
/rgm packs list        # which packs loaded, and why one failed
/rgm reload            # hot reload packs without a restart
```

317 admin commands across 72 modules, each with a permission node under `rgm.*`.

## What's in the box

| | |
|---|---|
| Modules | 72, across 11 layers |
| Domain events | 114 |
| Content types | 40 |
| Component schemas | 18 |
| Tests | 1,901 (unit, integration, functional, e2e, regression) |

Five architectural laws hold the design together:

1. **One source of truth** — `UniversalRegistry` for static content, `GameDataStore` for runtime
   state, `PersistenceSystem` as the only module that touches disk or SQL.
2. **Composition over inheritance** — a Boss and an NPC are the same entity class with different
   components attached.
3. **Events only** — modules talk through the EventBus, never by calling each other.
4. **One damage pipeline** — 16/17 are the only damage arithmetic; PvP adds rules on top.
5. **Data-driven** — every content type has a schema, and server settings ship as `module_config`
   pack content rather than Kotlin constants.

## Source

The full source, tests and design documents live in the private
[`iobsifox/RPGMaker-Source`](https://github.com/iobsifox/RPGMaker-Source) repository.
This repository ships release jars only.

## Requirements

- Paper 1.21.x
- Java 21
- No other plugins required
