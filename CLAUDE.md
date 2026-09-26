# CLAUDE.md

Before doing anything else in this repo, read the OpenSpec docs to get context:
1. `openspec/project.md` — purpose, tech context, conventions, open questions.
2. The relevant specs in `openspec/specs/` (e.g. `hardware/`, `input-devices/`).

Rules:
- Specs are the source of truth. Hardware and setup facts live in
  `openspec/specs/`, not in this file.
- Propose behaviour changes as OpenSpec change proposals under
  `openspec/changes/<change-name>/` (proposal, tasks, spec deltas) — use
  `/opsx:propose` — before implementing them.
- When something changes, update the specs (archive the change with
  `/opsx:archive`, or edit `openspec/specs/` directly for simple fact corrections).
- Don't guess unknown hardware details; mark them TBD in the specs.
- Device files target `/userdata` on the Batocera host.
