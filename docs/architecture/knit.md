---
sidebar_position: 4
title: Knit Service Setup
description: Integrating with Sleitnick's Knit framework
---

# Knit Service Architecture

The Knit Service format packages the Loadout System as a native Knit service using `Knit.CreateService`.

## Directory Structure

```text
ServerScriptService
└── Services
    └── LoadoutService (ModuleScript)
        ├── Types (ModuleScript)
        ├── Trove (ModuleScript)
        └── Utils (ModuleScript)
```

## Lifecycle Integration

- **`KnitInit()`**: Pre-boot lifecycle hook.
- **`KnitStart()`**: Gathers configured group IDs, registers `PlayerAdded`, `PlayerRemoving`, and `CharacterAdded` listeners, and initiates rank caching.

## Usage

Place `LoadoutService` in your Knit services folder. During Knit startup, Knit automatically runs `KnitInit` and `KnitStart`:

```luau
local Knit = require(game:GetService("ReplicatedStorage").Packages.Knit)

-- Register services
for _, service in game:GetService("ServerScriptService").Services:GetChildren() do
    if service:IsA("ModuleScript") then
        require(service)
    end
end

-- Start Knit
Knit.Start():andThen(function()
    print("Knit server started successfully")
end)
```
