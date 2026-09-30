---
sidebar_position: 2
title: Drop-In Setup
description: Setting up the self-contained Drop-In architecture
---

# Drop-In Architecture

The Drop-In format is a self-contained folder that runs automatically when placed in Roblox's `ServerScriptService`.

## Directory Structure

```text
ServerScriptService
└── LoadoutSystem
    ├── Handler.server.luau
    ├── LoadoutSystem.luau
    ├── Types.luau
    └── Handler
        ├── Trove.luau
        └── Utils.luau
```

## How It Works

1. `Handler.server.luau` listens to `Players.PlayerAdded` and `Players.PlayerRemoving`.
2. On `PlayerAdded`, it iterates through all configured group IDs in `LoadoutSystem.luau`, calls `GroupService:GetRolesInGroupAsync`, and caches the player's ranks.
3. On `CharacterAdded`, it resolves the player's team and rank, then applies inventory, clothes, stats, and morphs.
4. When a player leaves, a dedicated `Trove` object clears all cached roles and event listeners.

## Usage

Simply drag or sync the `LoadoutSystem` folder into `ServerScriptService`. Edit `LoadoutSystem.luau` to configure your team loadouts.
