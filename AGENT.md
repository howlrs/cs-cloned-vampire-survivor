# AGENT.md

## Project Snapshot
- **Name**: VampireSurvivorsClone – a top-down survival shooter targeting mobile and desktop.
- **Engine**: Unity 6000.2.6f1 (`ProjectSettings/ProjectVersion.txt`).
- **Key packages**: Input System, Localization, Timeline, AI Navigation, Visual Scripting, Unity Test Framework (see `Packages/manifest.json`).
- **Primary scenes**: `Assets/Scenes/Game/Level 1.unity`, `Assets/Scenes/Game/Main Menu.unity`, and the tooling scene `Assets/Scenes/Character Set Generation/Character Set Generation.unity`.

## Code & Gameplay Layout (`Assets/Scripts`)
- `Character/`: Player movement, stats, joystick input (`Character.cs`, `MainCharacter.cs`, `TouchJoystick.cs`).
- `Gameplay/`: Core loop (level/XP, inventory, pools, timers, damage interfaces). Object pools live in `Gameplay/Pools/`.
- `Monsters/` & `Projectiles/`: Enemy AI, spawn behaviours, projectile logic.
- `Collectables/` & `Throwables/`: Pickups, loot, consumables, throwable weapons.
- `ScriptableObjects/`: Blueprints for characters, levels, monsters, collectables. Expect designer configuration via inspector.
- `UI/`, `Localization/`, `Utilities/`, `Testing/`: HUD elements, localization helpers, shared helpers, and a lightweight `MiscTesting.cs` harness.

## Content & Data
- `Assets/Prefabs/`: Organised by category (abilities, monsters, projectiles, UI). Pools reference these prefabs.
- `Assets/Sprites`, `Assets/Textures`, `Assets/Fonts`, `Assets/Materials`: Visual assets; many sourced from Kenney/Bonsaiheldin.
- `Assets/AddressableAssetsData/`: Addressables profiles and group settings—keep in sync when adding new runtime-loaded assets.
- `Assets/Localization/`: Tables and locales enabling English/简体中文/繁體中文.
- `Assets/Input/`: Input System action assets for mobile/desktop controls.

## Getting Started
1. Open with Unity 6000.2.6f1 (Unity 6). Older 2021 docs in `README.md` are outdated.
2. Let packages reimport; do not version-control the `Library/` folder.
3. Load `Assets/Scenes/Game/Main Menu.unity` to access the playable loop; `Level 1.unity` loads from there.
4. For editor play mode, ensure the `EventSystem` and Input System action asset in the scene point to `Assets/Input/PlayerControls.inputactions`.

## Builds & Deployment
- Use the standard Build Settings; mobile presets are preconfigured in `ProjectSettings/` (Android/iOS profiles via Unity 6 mobile feature set).
- Addressables: run `Window > Asset Management > Addressables > Build > New Build > Default Build Script` after adding addressable assets.
- Verify ScriptableObject references in level/character blueprints before building to avoid missing asset links.

## Testing & Debugging
- Code-driven checks live in `Assets/Scripts/Testing/MiscTesting.cs`; extend with Unity Test Framework (`com.unity.test-framework`).
- Use the Pools inspector to monitor pool sizes; `StatsManager` and `LevelManager` expose runtime state in the editor.
- Logging relies on Unity's `Debug` API; no external logging framework is configured.

## Collaboration Notes
- Respect meta files (.meta) for asset GUID stability; version control should track them.
- Object pool and ScriptableObject relationships are inspector-driven—document new links in commits.
- Keep localisation keys consistent across tables; update `Assets/Localization` when UI text changes.
- Check in addressable content builds if required by CI, otherwise let the next agent rebuild locally.

## Useful Shortcuts
- Solution file: `VampireSurvivorsClone.sln` for IDE integrations (Rider/VS/VSCode packages included).
- Project settings: `ProjectSettings/InputManager.asset` (legacy) is unused; rely on `Input System` assets.
- Rapid iteration: duplicate entries under `Assets/Blueprints/` to create new abilities/enemies, then hook up via prefabs and scriptable objects.
