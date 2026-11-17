# Retreat-Forge

The game creation tool of the Retreat ecosystem: a desktop application where developers select modular game features, build a playable game from them, and then customize the generated project with AI assistance.

This file contains project facts and rules only. Personal preferences belong in `CLAUDE.local.md`.

The root `../CLAUDE.md` also applies here. This file may narrow its scope (see "Status") but not relax its rules; its verification and infrastructure rules apply only once this repository has code that needs them.

## Status

Early stage. No code exists yet. Do not implement anything beyond what is explicitly requested.

## Ecosystem context

Sibling repositories (cloned next to this one under `Retreat/`): `Retreat-CoreSDK`, `Retreat-Engine-3D`, `Retreat-Modules`, `Retreat-Website`. Vision documents live in the parent folder: `FRAMEWORK.en.md` and `PLATFORM.en.md`. Read `FRAMEWORK.en.md` (especially "Recorded Technical Decisions") before making architectural decisions.

## Decided direction

- **Desktop application**, likely Tauri (Rust backend, TypeScript UI). Distributed through GitHub Releases as installers for Windows, macOS, and Linux. Cloning the repository is for contributors.
- **Bundles the Rust toolchain**, so developers install nothing else.
- **Installer and launcher role (similar to Unreal's launcher):** Forge installs engine and module versions once into a central location on the developer's machine, with readable source, and updates them independently of Forge itself.
- **Projects are small and reference installed versions.** A project contains the developer's own game code and data, a project file naming the engine version and selected modules, and an AI context file. Developers choose where the project lives. Projects must not contain copies of the engine or modules by default.
- **Module resolution:** Forge reads each module's manifest (capabilities, dependencies, conflicts, configuration), validates the selection, and explains incompatibilities in plain language. Cargo only compiles; it is not the plugin system.
- **Build:** selecting features and clicking Build must produce a playable game, not an empty skeleton. Native (server) and WASM (client) targets are both built.
- **AI customization** happens after the first Build, at the lightest level that fits: (1) the game's own code and data; (2) a project-local override of a module, clearly marked in the project file; (3) a named custom engine version. Levels 2 and 3 are exceptions, and the agent explains what the developer now owns.
- **Safeguards for AI changes:** every change is a checkpoint (git commit), is validated by compiling and a smoke run before being accepted, and must not touch currency, balances, or ownership validation.

## Open decisions (do not assume an answer)

- Tauri versus another desktop framework.
- Where the AI agent runs: inside Forge or in the developer's own coding tool pointed at the project.
- Whether the agent may create module overrides without asking (current leaning: ask first).
- Build latency strategy (warm caches, prebuilt artifacts) and signing/notarization of installers.
- Release process and distribution of engine and module versions (crates.io, private registry, or git).

## Working rules

- Keep the initial implementation as simple as reasonably possible.
- Do not modify sibling repositories from this one without being asked.
- Write documentation and code comments in English.
