---
sidebar_position: 3
title: Module Setup
description: Integrating the standalone Module architecture
---

# Module Architecture

The Module format encapsulates the Loadout System inside a single ModuleScript object, designed for server architectures with centralized bootstrap scripts.

## Directory Structure

```text
ServerScriptService
└── LoadoutSystem (ModuleScript)
    ├── Types (ModuleScript)
    ├── Trove (ModuleScript)
    └── Utils (ModuleScript)
```

## Public API

### `LoadoutSystem.Settings`
The configuration table containing `DebugMode`, `ToolStorage`, and `Loadouts`. Can be configured directly inside the module or overwritten at runtime prior to calling `:Start()`.

### `LoadoutSystem:Start()`
Initializes group discovery, hooks into player and character lifecycle events, and processes all existing players currently in the game.

## Initialization Example

```luau
-- Server bootstrap script (e.g. ServerScriptService.Starter)
local LoadoutSystem = require(game:GetService("ServerScriptService"):WaitForChild("LoadoutSystem"))

-- Optionally modify settings programmatically:
LoadoutSystem.Settings.DebugMode = true

-- Start the system
LoadoutSystem:Start()
```
