---
sidebar_position: 1
title: Morph System Guide
description: Rigging, model structure, and automatic welding mechanics
---

# Morph Configuration Guide

The morph system enables games to attach custom 3D armor, tactical vests, helmets, belts, and limb attachments onto player characters with precise positioning and automated physics handling.

## How the Welder Works

1. **Cloning**: The specified `Morph` model is cloned when a player spawns.
2. **Limb Matching**: The welder iterates through each child in the clone that is a `BasePart`. This part is treated as a reference limb (e.g. `Head`, `UpperTorso`, `LeftUpperArm`).
3. **Limb Verification**: It searches the player's character for a limb matching the same name.
4. **Armor Attachment**: For each item (BasePart, Model, Folder, or Accessory) inside that reference limb:
   - Destroys any preexisting joints (`JointInstance` or `WeldConstraint`) to prevent rigid physics fighting.
   - Calculates the relative CFrame offset: `offset = referenceLimb.CFrame:Inverse() * armorPart.CFrame`.
   - Positions the part on the player: `armorPart.CFrame = playerLimb.CFrame * offset`.
   - Parents the item to the player limb.
   - Sets physics flags: `Anchored = false`, `CanCollide = false`, `Massless = true`.
   - Creates a `WeldConstraint` linking the character limb (`Part0`) to the armor part (`Part1`).
5. **Cleanup**: Destroys the cloned template dummy, leaving only the attached armor parts on the character.

---

## Studio Rigging Instructions

### 1. Build Around a Reference Dummy
- Insert an R15 or R6 rig in Studio (matching your game's avatar type).
- Group the rig into a Model (e.g. `R15_OfficerMorph`).

### 2. Nest Armor Under Limb Parts
- Position your armor parts on the dummy visually.
- Group each armor piece under the corresponding limb BasePart in the dummy.

```text
R15_OfficerMorph (Model)
├── Head (BasePart: reference frame)
│   └── Helmet (Model)
│       └── HelmetMesh (MeshPart)
├── UpperTorso (BasePart: reference frame)
│   ├── Vest (Model)
│   │   ├── FrontPlate (MeshPart)
│   │   └── Pouches (MeshPart)
│   └── Radio (BasePart)
├── LeftUpperArm (BasePart: reference frame)
│   └── ShoulderPad (MeshPart)
└── RightUpperArm (BasePart: reference frame)
    └── ShoulderPad (MeshPart)
```

### 3. Rules & Best Practices
- **Do not rename reference limbs**: Limb names must match standard character part names (`UpperTorso`, `Head`, `LeftUpperArm`, etc. in R15; `Torso`, `Head`, etc. in R6).
- **No manual welds required**: Leave the parts unanchored or anchored; the system strips conflicting welds and creates clean `WeldConstraint` instances.
- **Reference limbs are discarded**: Only the children of the reference limb are parented to the player, so you do not need to make the dummy invisible.
- **Storage**: Store morph models in `ServerStorage` or `ReplicatedStorage`, and reference them in your configuration table.
