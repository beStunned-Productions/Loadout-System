---
sidebar_position: 1
title: Overview & Settings
description: Configuring teams, groups, and rank resolution rules
---

# Configuration Overview

Configurations are located in `Settings` (`LoadoutSystem.luau` or `LoadoutService.luau`).

## Settings Structure

```luau
local Settings: Types.Settings = {
    DebugMode = false,
    ToolStorage = nil,
    Loadouts = {
        ["TeamName"] = {
            DefaultLoadout = { ... },
            ["GroupId"] = {
                ["RankId"] = { ... },
            },
        },
    },
}
```

## General Settings

### `DebugMode`
- Type: `boolean`
- Default: `false`
- Description: When enabled, logs detailed messages regarding player cache creation, group roles fetched, rank resolution, tool lookups, and asset loads.

### `ToolStorage`
- Type: `(Instance | { Instance })?`
- Default: `nil`
- Description: Designates where the system looks for named tools and accessories.
  - If set to an Instance (e.g. `ServerStorage.Guns`), tools are searched recursively within that folder.
  - If set to a table of Instances, the system checks each container sequentially.
  - If set to `nil`, defaults to searching `ServerStorage` and `ReplicatedStorage`.

---

## Loadout Resolution Order

When a player spawns, the system resolves their loadout using these steps:

1. **Team Check**: Matches `player.Team.Name` against keys in `Settings.Loadouts`. If no match exists, no loadout is granted.
2. **Group Hierarchy**: Checks every group ID configured under that team.
   - For each group the player is in, tests configured rank IDs.
   - **`Exact` match**: Rank must equal the configured ID.
   - **`Minimum` match**: Rank must be greater than or equal to the configured ID.
   - If multiple ranks match, the highest numerical rank takes priority.
3. **Fallback**: If no group rule matches, the team's `DefaultLoadout` is applied.
