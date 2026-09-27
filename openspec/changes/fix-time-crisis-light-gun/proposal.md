## Why

Time Crisis would not play properly with the Sinden Light Gun (issue #5).
Three separate problems stood in the way:

- The `timecris` ROM set was incomplete. It also needs the device ROM zip
  `namcoc71.zip` (`c71.bin`, the Namco C71 MCU) in `/userdata/roms/mame`.
- Batocera launched it with the MAME 2003-Plus libretro core
  (`mame078plus`), which has no driver for it ("Game driver not found for
  timecris"). It needs Batocera's standalone MAME (0.285, `/usr/bin/mame/mame`).
- On standalone MAME the crosshair sat half a screen to the left. Batocera's
  generated MAME input config binds Lightgun X to
  `Gun 1 axis X or Mouse X or Joy 1 A1` (and Y to `... Joy 1 A2`). MAME adds
  together absolute inputs joined with OR. MAME's Joy 1 is an arcade kit
  encoder (TS-UAIB-OP02), which reads full-left until it is touched (the same
  quirk as issue #2), so it adds minus half a screen to the gun's aim. The
  Sinden itself was fine: `evtest` showed ABS_X/ABS_Y 0..32767 with the full
  range used.

Time Crisis II, 3 and 4 were also checked. They are flagged
`MACHINE_NOT_WORKING` in MAME itself, so they are out of scope here.

## What Changes

- Ship a per-game MAME config at
  `/userdata/system/configs/mame/timecris.cfg` that binds the P1 light gun X
  and Y axes to the Sinden gun only (`GUNCODE_1_XAXIS` / `GUNCODE_1_YAXIS`),
  with no mouse or joystick axis mixed in.
- Document two manual on-device steps (the repo does not track
  `batocera.conf` or ROMs):
  - Run `timecris` on standalone MAME: in `batocera.conf` set
    `mame["timecris.zip"].emulator=mame` and `mame["timecris.zip"].core=mame`
    (or EmulationStation > game > Advanced game options > Emulator).
  - Add `namcoc71.zip` to `/userdata/roms/mame` and check the set with
    `/usr/bin/mame/mame -rompath /userdata/roms/mame -verifyroms timecris`.

## Capabilities

### New Capabilities
<!-- none -->

### Modified Capabilities
- `input-devices`: new requirements that in MAME light gun games the aim
  comes only from the Sinden, and that Time Crisis (`timecris`) runs on
  standalone MAME and is playable with the Sinden.

## Impact

- New device file under `userdata/system/configs/mame/` (deployed to the same
  path under `/userdata`).
- Only `timecris` is affected. Other MAME light gun games keep Batocera's
  generated bindings (see design.md for the follow-up).
- Manual per-game emulator setting in `batocera.conf` and an extra ROM file
  on the device; neither is stored in the repo.
