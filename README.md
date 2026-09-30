# Loadout System

A server-side role and team-based loadout management system for Roblox. It automates inventory provisioning, humanoid statistics, clothing, accessory loading, morph welding, custom character attributes, and collision groups based on a player's team and group rank.

**Documentation Website**: [https://bestunned-productions.github.io/Loadout-System/](https://bestunned-productions.github.io/Loadout-System/)

---

## Key Features

- **Multi-Architecture Support**: Built for three common project structures: Drop-In (zero setup), Module (for custom server frameworks), and Knit Service (for Sleitnick's Knit).
- **Group & Rank Resolution**: Supports exact rank matching (`"Exact"`) and rank hierarchy thresholds (`"Minimum"`), with automatic fallback to team defaults.
- **Optimized Performance**: Queries group roles once on player join via `GroupService:GetRolesInGroupAsync` and caches results, eliminating network throttle delays during character respawns.
- **Leak-Free Lifecycle**: Uses `Trove` to clean up player connections and cached data automatically upon disconnect.
- **Automated Morph Welder**: Attaches custom 3D armor, vests, and accessories with relative CFrame inverse offset calculation, auto-generated `WeldConstraint` joints, and non-colliding massless parts.
- **Full Character Customization**: Controls humanoid stats, body scaling (`HumanoidDescription`), clothing overrides, skin tones, collision groups, and custom attributes.

---

## Architectures

| Architecture | Path | Best Used For |
| :--- | :--- | :--- |
| **Drop-In** | `Drop-In/LoadoutSystem` | Plug-and-play setup. Place in `ServerScriptService` and run without extra scripts. |
| **Module** | `Module/LoadoutSystem` | Games using custom server loaders. Initialized via `LoadoutSystem:Start()`. |
| **Knit Service** | `Knit/LoadoutService` | Games using the Knit framework. Starts via `KnitStart()`. |

---

## Quick Configuration Example

Configurations are defined in `Settings` (`LoadoutSystem.luau` or `LoadoutService.luau`):

```luau
local Types = require(script.Types)

local Settings: Types.Settings = {
    DebugMode = false,
    ToolStorage = game:GetService("ServerStorage"):WaitForChild("Weapons"),

    Loadouts = {
        ["Security"] = {
            DefaultLoadout = {
                ClearInventoryOnSpawn = true,
                Health = 100,
                Movement = {
                    WalkSpeed = 16,
                    JumpPower = 50,
                },
                Clothing = {
                    Shirt = "rbxassetid://123456789",
                    Pants = "rbxassetid://987654321",
                },
                Tools = {
                    "Taser",
                    "Handcuffs",
                },
                CollisionGroup = "Players",
            },

            -- Group ID as a string key
            ["34567890"] = {
                -- Rank 10 and above (Minimum match mode)
                ["10"] = {
                    ClearInventoryOnSpawn = true,
                    RankMatchMode = "Minimum",
                    Health = 125,
                    Movement = {
                        WalkSpeed = 18,
                        JumpPower = 50,
                    },
                    Attributes = {
                        Armor = 50,
                        Role = "Patrol Officer",
                    },
                    Clothing = {
                        Shirt = "rbxassetid://11112222",
                        Pants = "rbxassetid://33334444",
                    },
                    RemoveAccessories = { Enum.AccessoryType.Hat, Enum.AccessoryType.Hair },
                    Accessories = {
                        "PatrolCap",
                    },
                    Tools = {
                        "Sidearm",
                        "Taser",
                        "Flashlight",
                    },
                    CollisionGroup = "Players",
                },

                -- Rank 255 (Exact match mode)
                ["255"] = {
                    ClearInventoryOnSpawn = true,
                    RankMatchMode = "Exact",
                    Health = {
                        MaxHealth = 175,
                        Health = 175,
                    },
                    Movement = {
                        WalkSpeed = 20,
                        JumpPower = 50,
                    },
                    Attributes = {
                        Armor = 100,
                        Role = "Commander",
                    },
                    RemoveAccessories = true,
                    Clothing = {
                        Shirt = "rbxassetid://55556666",
                        Pants = "rbxassetid://77778888",
                    },
                    Morph = game:GetService("ServerStorage"):WaitForChild("Morphs"):WaitForChild("CommanderRig"),
                    Tools = {
                        "Sidearm",
                        "AssaultRifle",
                        "Handcuffs",
                        "Radio",
                    },
                    CollisionGroup = "Players",
                    OnSpawn = function(player: Player, character: Model, loadout: Types.RankLoadout?)
                        local overhead = game:GetService("ServerStorage"):WaitForChild("RankUI"):Clone()
                        overhead.Parent = character:WaitForChild("Head")
                    end,
                },
            },
        },
    },
}

return table.freeze(Settings)
```

---

## Documentation

The full documentation website is published at [https://bestunned-productions.github.io/Loadout-System/](https://bestunned-productions.github.io/Loadout-System/).

It is built with Moonwave and organized into the following sections in `docs/`:

- [System Architectures](docs/architecture/index.md)
  - [Drop-In Architecture](docs/architecture/drop-in.md)
  - [Module Architecture](docs/architecture/module.md)
  - [Knit Service Architecture](docs/architecture/knit.md)
- [Configuration Guide](docs/configuration/index.md)
  - [Settings and Group Resolution](docs/configuration/index.md)
  - [RankLoadout Properties Reference](docs/configuration/properties.md)
- [Morph System Guide](docs/morphs/index.md)
  - [Rigging, Welding Mechanics and Model Hierarchy](docs/morphs/index.md)
- [Troubleshooting](docs/troubleshooting/index.md)
  - [Common Issues and Debugging](docs/troubleshooting/index.md)

---

## Support & Bug Reports

If you run into issues while configuring your loadouts or discover a bug in the system, you can request support or submit a bug report through our Discord community:

- **Discord Server**: [https://discord.gg/3W436mHzr2](https://discord.gg/3W436mHzr2)

---

## Building Documentation Locally

This project uses [Moonwave](https://eryn.io/moonwave) to build documentation:

```bash
# Preview documentation in development mode
moonwave dev

# Build static documentation files
moonwave build
```

