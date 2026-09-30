---
sidebar_position: 1
title: Overview
description: Overview of the three architecture styles
---

# System Architectures

The Loadout System is distributed in three distinct project structures to accommodate different game architectures.

## Architecture Comparison

| Architecture | Folder Location | Lifecycle Driver | Recommended Use |
| :--- | :--- | :--- | :--- |
| **Drop-In** | `Drop-In/LoadoutSystem` | `Handler.server.luau` | Standalone games or quick implementation without existing frameworks. |
| **Module** | `Module/LoadoutSystem` | `LoadoutSystem:Start()` | Custom server bootstrap loaders and centralized architecture patterns. |
| **Knit Service** | `Knit/LoadoutService` | `KnitStart()` | Projects built on Sleitnick's Knit framework. |

## Shared Core Mechanics

All three implementations share identical internal mechanics:
- Role lookup caching using `GroupService:GetRolesInGroupAsync` on `PlayerAdded`.
- `Trove` per player for memory cleanup on `PlayerRemoving`.
- Dynamic CFrame offset calculation and welding for character morphs.
- `HumanoidDescription` application for body colors and scale modifiers.
