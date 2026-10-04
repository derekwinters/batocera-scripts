## Why

Issue #6 asked how to calibrate the Sinden Light Gun and whether the
calibration survives a reboot. While checking that, a second problem showed
up: with a Sinden connected, Batocera drew the white Sinden border around
every game, not just light gun games.

The gun-related settings live only in `/userdata/system/batocera.conf` on the
arcade, which the repo does not track, so there was no record of how the gun
is set up.

## What Changes

- Add `userdata/system/batocera-lightgun.conf`: a commented copy of the
  owner's gun-related `batocera.conf` lines (crosshairs, Time Crisis
  emulator/gun settings, and the Sinden border settings). Batocera does not
  load this file; its lines are kept in sync with `batocera.conf` by hand
  until automated backup/restore (issue #12) exists.
- Hide the Sinden border globally (`controllers.guns.bordersmode=hidden`) and
  turn it back on per gun game (`<system>["<rom>"].bordersmode=normal`).
- Remove the system-wide `mame.use_guns=1`, which forced gun mode on every
  MAME game. Batocera already enables gun mode for games flagged as gun games.
- Document where the Sinden calibration is stored (on the gun itself) and
  that it survives reboots.

## Capabilities

### New Capabilities
<!-- none -->

### Modified Capabilities
- `input-devices`: the "Sinden Light Gun support" requirement now says the
  border is shown only in listed gun games, never in other games even with
  the gun attached, and that calibration is stored on the gun and persists
  across reboots.

## Impact

- New reference file under `userdata/system/`. It is not deployed as-is; its
  lines go into `/userdata/system/batocera.conf` on the arcade.
- Adding a new gun game means adding one `bordersmode=normal` line for it.
