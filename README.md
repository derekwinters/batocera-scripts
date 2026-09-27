# Batocera Scripts and Config

Scripts and configuration for a home arcade running [Batocera](https://batocera.org/).

Project context and specs are managed with [OpenSpec](https://github.com/Fission-AI/OpenSpec):

- `openspec/project.md` — purpose, tech context, conventions
- `openspec/specs/` — current setup (hardware, input devices)
- `openspec/changes/` — proposed changes

## Deploying

Files under `userdata/` mirror their location on the device: copy them to the
same path under `/userdata` on the Batocera host. Services in
`userdata/system/services/` must be executable and are enabled with
`batocera-services enable <name>` (or System Settings > Services).
