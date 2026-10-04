## Context

Facts checked against Batocera's source and the owner's live config
(2026-10-04).

### Calibration storage

The udev script `virtual-sindenlightgun-add` copies Batocera's Sinden config
template (`/usr/share/sinden/LightgunMono.exe.config`) to
`/var/run/sinden/p<hash>/`. That is tmpfs and is rebuilt on every boot, so no
calibration is kept there. The template sets
`AutoSaveCalibrationInLightgun=1`, so the Sinden software saves calibration on
the gun itself, and it survives reboots.

Batocera's separate `batocera-gun-calibrator` stores data in
`/userdata/system/configs/gun-calibrator/`. That directory does not exist on
this arcade, so it is not used for the Sinden.

### Border

In Batocera's launcher (`batocera_launch/emulator.py`, `guns_borders_size`),
the default `bordersmode` is `auto`. In `auto` the Sinden border is drawn in
every game whenever a Sinden is connected, not just gun games. The border is
needed in gun games because the Sinden tracks the white frame.

### Gun mode

Batocera turns on gun mode (passes `-lightgun`) for games flagged as gun games
in EmulationStation's `/usr/share/emulationstation/resources/gamesdb.xml`.
The system-wide `mame.use_guns=1` forced gun mode on every MAME game, so it
was removed. Time Crisis keeps a per-game `use_guns=1`.

### MAME crosshair

`mame_crosshair` takes `disabled`, `enabled` or `onmove`. `enabled` always
shows the crosshair; `onmove` leaves it to MAME, which shows it only while the
gun moves.

## Decisions

- **Border off by default, on per game.** Set
  `controllers.guns.bordersmode=hidden` globally and
  `<system>["<rom>"].bordersmode=normal` for each gun game. The per-game key
  overrides the global one. The cost is one line per new gun game.
- **Keep the settings in a reference file.** `batocera.conf` is not tracked,
  so the gun lines are kept in `userdata/system/batocera-lightgun.conf` and
  copied by hand until backup/restore (issue #12) automates it.
- **No calibration files in the repo.** Calibration lives on the gun, so
  there is nothing under `/userdata` to back up for it.

## Risks / Trade-offs

- A gun game without a `bordersmode=normal` line gets no border, and the
  Sinden cannot track the screen. Fix: add the line.
- The reference file can drift from the live `batocera.conf` until issue #12
  is done.
