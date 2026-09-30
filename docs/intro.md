---
sidebar_position: 1
title: Introduction
description: Overview of the Loadout System
---

# Introduction

The Loadout System is a modular, high-performance role and team-based inventory and character management system for Roblox. It automates inventory distribution, character attributes, clothing, accessories, morph welding, and physics setup upon player spawn.

## Why Use This System?

- **Team and Group Hierarchy**: Map loadouts directly to Teams, Groups, and Group Ranks.
- **Multiple Integration Styles**: Works out of the box with Drop-In, standalone Module scripts, or Sleitnick's Knit framework.
- **Zero Spawn Lag**: Pre-caches group rank memberships on player join, avoiding repeated Roblox API queries during respawn.
- **Automated Morph Assembly**: Attaches complex 3D armor models to R15 and R6 character rigs automatically with calculated CFrame offsets and rigid constraint joints.
- **Clean Memory Lifecycle**: Trove cleans up all event connections, tables, and cached data when players disconnect.

## Next Steps

- Explore the available integration formats in [System Architectures](architecture/index.md).
- Learn how to configure teams and ranks in [Configuration Guide](configuration/index.md).
- Check the full list of supported settings in [RankLoadout Properties](configuration/properties.md).
- Learn how to set up custom 3D armor rigs in [Morph System Guide](morphs/index.md).
