# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Unity 3D multiplayer modification for Daggerfall Unity v0.14.5. The project adds multiplayer functionality to the open-source recreation of The Elder Scrolls II: Daggerfall using Mirror networking and Steam integration.

**Unity Version**: 2019.4.40f1 (LTS) - This specific version is required for compatibility.

## Architecture

### Core Components

1. **Base Game**: Daggerfall Unity implementation in `/Assets/Scripts/`
2. **Multiplayer Mod**: Located in `/Assets/Game/Mods/TanguyMultiplayer/`
   - Uses Mirror networking framework with FizzySteamworks transport
   - Synchronizes player positions, entity states, doors, loot, weather, and time
   - 8-directional sprite rendering for multiplayer characters

### Key Systems

- **Networking**: Mirror framework handles client-server communication
- **Steam Integration**: Steamworks.NET provides lobby and matchmaking
- **Asset Management**: Unity Addressables system for efficient loading
- **Mod System**: DFU's modding framework with .dfmod packaging
- **Synchronization**: "Catcher" components monitor and sync game state:
  - `EntityCatcher.cs` - Enemy entities in dungeons
  - `DoorsCatcher.cs` - Door open/closed states
  - `LootCatcher.cs` - Loot container contents
  - `TimeCatcher.cs` - Game time
  - `WeatherCatcher.cs` - Weather conditions

## Development Commands

### Building the Project

1. **Unity Build**:
   - Open project in Unity 2019.4.40f1
   - File → Build Settings → Select platform → Build
   - Post-build processing automatically cleans debug files

2. **Mod Package Creation**:
   - In Unity: Daggerfall Tools → Mod Builder
   - Builds .dfmod file from mod contents

### Testing

1. **Unity Editor Testing**:
   - Open `DaggerfallUnityGame.unity` scene
   - Press Play to test in editor
   - For multiplayer: Ensure Steam is running (uses test app ID 480)

2. **Unity Test Framework**:
   - Window → General → Test Runner
   - Run available unit tests (primarily in Mirror components)

### Development Workflow

1. **Code Location**:
   - Multiplayer scripts: `/Assets/Game/Mods/TanguyMultiplayer/Scripts/`
   - Core game modifications: Avoid unless necessary
   - Editor tools: `/Assets/Game/Mods/TanguyMultiplayer/Scripts/Editor/`
   - Mirror framework: `/Assets/Game/Mods/TanguyMultiplayer/Scripts/Mirror/` (bundled)

2. **Asset References**:
   - Prefabs: `/Assets/Game/Mods/TanguyMultiplayer/Prefabs/`
   - Use Unity's prefab system for networked objects
   - Key prefabs: `NetworkManager.prefab`, `PlayerMultiplayer.prefab`

3. **Networking Patterns**:
   - Inherit from NetworkBehaviour for synchronized components
   - Use [Command] for client-to-server calls
   - Use [ClientRpc] for server-to-client calls
   - Use [SyncVar] for automatically synchronized variables
   - Follow the "Catcher" pattern for synchronizing existing game systems

## Important Files

- `tanguysmultiplayer.dfmod.json` - Mod manifest and configuration
- `NetworkManager.prefab` - Core multiplayer setup
- `ModInitiator.cs` - Main mod entry point (uses DFU's [Invoke] system)
- `MultiplayerManager.cs` - Multiplayer initialization and HUD management
- `PlayerMultiplayer.cs` - Networked player component
- `SteamLobby.cs` - Steam integration and lobby system
- `GameManager.cs` - Core game state management (avoid modifying)

## Common Tasks

### Adding New Synchronized Features

1. Create NetworkBehaviour component
2. Add to appropriate game object via mod initialization
3. Register with Mirror's spawning system if needed
4. Test with multiple Unity instances (ParrelSync recommended)

### Debugging Multiplayer

1. Enable Mirror debug logging in NetworkManager
2. Use Unity's Console for network messages
3. Test with local builds before Steam integration

## Constraints

- Maintain compatibility with base Daggerfall Unity v0.14.5
- Preserve save game compatibility where possible
- Follow Unity's component-based architecture
- Use existing DFU systems (quest, spell, item) rather than reimplementing