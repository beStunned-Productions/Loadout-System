---
sidebar_position: 2
title: RankLoadout Properties
description: Comprehensive reference for all RankLoadout options
---

# RankLoadout Property Reference

Each loadout definition inside `DefaultLoadout` or under a specific `RankId` accepts the following fields:

## Inventory & Items

### `ClearInventoryOnSpawn`
- Type: `boolean?`
- Description: When `true`, empties the player's `Backpack` and destroys any equipped `Tool` instances before granting new items.

### `Tools`
- Type: `{ (Tool | string) }?`
- Description: Array of tools to give to the player.
  - Can be direct `Tool` instances.
  - Can be tool names as strings, which are searched for recursively in `Settings.ToolStorage`.

---

## Character Stats & Movement

### `Health`
- Type: `(number | { MaxHealth: number?, Health: number? })?`
- Description: Configures Humanoid health.
  - If a number is provided (e.g. `150`), both `MaxHealth` and `Health` are set to that value.
  - If a table is provided, you can set `MaxHealth` and `Health` independently.

### `Movement`
- Type: `MovementModifiers?`
- Supported fields:
  - `WalkSpeed: number?`: Sets `humanoid.WalkSpeed`.
  - `JumpPower: number?`: Sets `humanoid.JumpPower`.
  - `JumpHeight: number?`: Sets `humanoid.JumpHeight`.
  - `UseJumpPower: boolean?`: Toggles whether the humanoid uses JumpPower or JumpHeight.

---

## Clothing & Accessories

### `Clothing`
- Type: `ClothingOverrides?`
- Supported fields:
  - `Shirt`: Asset ID string (e.g. `"rbxassetid://123456"`) or a `Shirt` instance.
  - `Pants`: Asset ID string or a `Pants` instance.
  - `TShirt`: Graphic asset ID string for a `ShirtGraphic`.

### `Accessories`
- Type: `{ string }?`
- Description: List of accessories to grant:
  - Numeric strings (e.g. `"12345678"`) are loaded via `InsertService:LoadAsset`.
  - Named strings (e.g. `"TacticalHelmet"`) are searched for inside `ToolStorage` as `Accessory` instances.

### `RemoveAccessories`
- Type: `(boolean | Enum.AccessoryType | { Enum.AccessoryType })?`
- Description: Strips existing accessories on character spawn:
  - `true`: Strips all accessories via `humanoid:RemoveAccessories()`.
  - An `Enum.AccessoryType` (e.g. `Enum.AccessoryType.Hair`): Removes only accessories of that specific category.
  - Table of `Enum.AccessoryType`: Removes accessories matching any of the specified types.

---

## Body Customization

### `CharacterColors` / `BodyColors`
- Type: `(Color3 | BrickColor | CharacterColorsModifiers)?`
- Description: Updates skin tone via `HumanoidDescription`.
  - Can be a single `Color3` or `BrickColor` applied to all body parts.
  - Can be a table specifying limb colors: `HeadColor`, `TorsoColor`, `LeftArmColor`, `RightArmColor`, `LeftLegColor`, `RightLegColor`.

### `Size`
- Type: `SizeModifiers?`
- Description: Adjusts R15 rig dimensions via `HumanoidDescription`:
  - `Scale`: Uniform body scale factor.
  - `BodyDepthScale`, `BodyHeightScale`, `BodyWidthScale`: Directional scaling.
  - `HeadScale`: Head scale factor.
  - `BodyProportionScale`: Proportion scale.
  - `BodyTypeScale`: Body type blend factor.

---

## Morphs & Physics

### `Morph`
- Type: `Model?`
- Description: Reference to a template morph model. The system clones the model, matches limb names, computes relative CFrame offsets, and welds armor pieces directly to the character limbs.

### `CollisionGroup`
- Type: `string?`
- Description: Sets the `CollisionGroup` property on every `BasePart` in the character model.

---

## Attributes & Hooks

### `Attributes`
- Type: `{ [string]: AttributeValue }?`
- Description: Key-value attributes assigned to the character model via `character:SetAttribute(name, value)`. Useful for armor values, roles, stamina, or overhead tag data.

### `OnSpawn`
- Type: `(player: Player, character: Model, loadout: RankLoadout?) -> ()?`
- Description: Custom callback executed asynchronously via `task.spawn` after the loadout has been applied.
