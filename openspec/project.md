# Project Context

## Purpose
Scripts and configuration for a home arcade cabinet running Batocera Linux.
This repo is the source of truth for how the arcade is set up and how it
should behave: hardware inventory, controller/player mapping, emulator
configuration, and any helper scripts deployed to the device.

## Tech Context
- **OS:** Batocera Linux (retro-gaming distribution) on an x86_64 mini PC.
- **Frontend:** EmulationStation (Batocera's fork).
- **Emulation:** mostly RetroArch / libretro cores, plus standalone emulators
  that Batocera ships.
- **User data on the device:** everything persistent lives under `/userdata`:
  - `/userdata/system/batocera.conf` — main system and per-system/per-game settings
  - `/userdata/system/scripts/` — event scripts (game start/stop hooks)
  - `/userdata/system/services/` — user services
  - `/userdata/system/configs/` — emulator configs (e.g. `retroarch/`, `emulationstation/`)
  - `/userdata/roms/`, `/userdata/bios/`, `/userdata/saves/`
- The rest of the root filesystem is read-only/overwritten on upgrade, so all
  customisation MUST target `/userdata`.

## Where Things Live
- `openspec/specs/` — current truth: what the arcade has and how it works today.
  - `hardware/` — host, display, attached devices.
  - `input-devices/` — controller-to-player mapping and light gun behaviour.
- `openspec/changes/` — proposals for how the arcade *should* work going
  forward. Each change is a folder with `proposal.md`, `tasks.md`, optional
  `design.md`, and spec deltas under `specs/`. Completed changes are archived
  to `openspec/changes/archive/` and merged into `openspec/specs/`.

## Conventions
- Hardware/setup facts belong in `openspec/specs/`, not in README or CLAUDE.md.
- Unknown facts are marked **TBD** and listed under Open Questions below rather
  than guessed.
- Repo paths for device files should mirror their `/userdata` location
  (e.g. `userdata/system/batocera.conf`) so deployment is a straight copy.
- Scripts target Batocera's shell (bash/busybox) and must be safe to re-run.
- Commit messages: short imperative summary, conventional prefixes
  (`docs:`, `feat:`, `fix:`, `chore:`) welcome.

## Open Questions
- Exact Minisforum model, CPU/GPU, RAM, storage layout (TBD).
- Batocera version installed (TBD).
- Exact models of the SNES, N64, and PlayStation-style USB controllers (TBD).
- Sinden Light Gun model/count (TBD) and how the border is provided (TBD).
- How the repo is deployed to the device (manual copy, rsync, git on device — TBD).
