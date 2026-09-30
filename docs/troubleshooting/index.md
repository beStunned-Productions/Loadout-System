---
sidebar_position: 1
title: Troubleshooting
description: Diagnosing and resolving common issues
---

# Troubleshooting Guide

## Enabling Debug Mode

Set `DebugMode = true` in your settings to view diagnostic logs in the server console:

```luau
local Settings: Types.Settings = {
    DebugMode = true,
    -- ...
}
```

When enabled, the system prints warnings when:
- A player's roles are fetched and cached.
- A player character is added and a loadout is matched.
- A requested tool or accessory cannot be found in storage.
- An accessory asset fails to load via InsertService.
- A player leaves and their cache is cleared.

---

## Common Issues

### 1. Tools are not appearing in the backpack
- **Check exact names**: Tool names in the `Tools` table are case-sensitive and must match the `Name` property of the Tool instance.
- **Check storage path**: If `ToolStorage` is set, confirm the tool is placed inside that container. If set to `nil`, ensure the tool is in `ServerStorage` or `ReplicatedStorage`.

### 2. Group ranks are not recognized
- **Check key types**: Ensure the group ID in `Settings.Loadouts` is a string (e.g. `["12345678"]`).
- **Check rank numbers**: Rank IDs must be valid integer strings between `1` and `255`.
- **API permissions**: In Roblox Studio local playtests, ensure game permissions allow API access if testing with live groups.

### 3. Morphs are offset or falling through the ground
- **Limb naming**: Ensure the reference limbs in your morph model match your game's rig type (R15 uses `UpperTorso`, `LowerTorso`, `Head`, etc.; R6 uses `Torso`, `Head`, etc.).
- **Neutral pose**: Assemble your morph model on a dummy in the default neutral pose so the relative CFrame offset matches the player's spawn pose.
- **Pre-existing welds**: Ensure armor parts do not have hardcoded motor joints or rigid welds that conflict with dynamic welding.

---

## Support & Bug Reports

If you encounter difficulties configuring the system or find a bug, you can request support or report the issue in our Discord:

- **Discord Community**: [https://discord.gg/3W436mHzr2](https://discord.gg/3W436mHzr2)

