## Context

Batocera starts standalone MAME with `-lightgunprovider udev
-lightgun_device lightgun` and `-cfg_directory /userdata/system/configs/mame`.
Its generated default input config binds each light gun axis to the gun, the
mouse and a joystick axis joined with OR, for example
`Gun 1 axis X or Mouse X or Joy 1 A1`.

For absolute axes, MAME adds the values of every OR'd input together
(`accumulate_axis_value` in `src/emu/input.cpp`: `result += value`). MAME's
Joy 1 on this arcade is an arcade kit encoder (TS-UAIB-OP02). Until it is
touched it reads its minimum (full left/up), which shifts the gun's aim by
half a screen. The Sinden reports correctly (`/dev/input/event19`, ABS_X and
ABS_Y 0..32767, full range used).

In the `timecris` driver (`namco/namcos22.cpp`, `INPUT_PORTS_START( timecris )`)
the gun ports are `:OPT.0` (X, default 68+626/2 = 381) and `:OPT.1` (Y,
default 43+241/2 = 163), both with mask `0xffff` (65535). MAME ignores a
per-game port override unless its tag, type, mask and default value all match
the driver.

## Goals / Non-Goals

**Goals:**
- Time Crisis aims from the Sinden only, whatever state the arcade kits are
  in.
- The fix lives under `/userdata` and survives Batocera upgrades.

**Non-Goals:**
- Time Crisis II, 3 and 4. They are `MACHINE_NOT_WORKING` in MAME
  (`timecrs2` in `namco/namcos23.cpp`; `timecrs3`/`timecrs4` in
  `namco/namcops2.cpp`, System 246/256). PS2 ports (PCSX2 with GunCon 2) are
  the likely future route for II and 3; 4 is PS3-only and out of scope.
- Fixing the aim for every MAME light gun game at once.
- Shipping ROMs or `batocera.conf` in the repo.

## Decisions

### Use a per-game MAME config instead of changing Batocera's generator

A per-game `timecris.cfg` in MAME's cfg directory overrides only the two gun
ports and binds them to `GUNCODE_1_XAXIS` / `GUNCODE_1_YAXIS`. It is a plain
file under `/userdata`, needs no change to Batocera's read-only config
generator, and was confirmed working on the device.

MAME rewrites the cfg file when the game exits, but it keeps the `<input>`
section, so the override stays in place.

Alternatives considered:
- *Change Batocera's MAME config generator so gun axes never include mouse or
  joystick.* This would fix every light gun game, but the generator is on the
  read-only root filesystem and would need an upstream change or a patch that
  is re-applied after upgrades. Kept as a follow-up.
- *Use the MAME 2003-Plus libretro core.* Not possible: it has no
  `timecris` driver.

## Risks / Trade-offs

- [Other MAME light gun games still have the mixed bindings and the same
  offset] → Add per-game configs as games are found, or make the durable
  all-games fix as a follow-up change.
- [A future MAME version could change the `timecris` port tags, masks or
  defaults, so MAME would silently ignore the override] → The on-device aim
  check will catch it; update the cfg values from the driver source.
- [The per-game emulator setting and `namcoc71.zip` are manual steps outside
  the repo] → Listed as on-device tasks.

## Open Questions

- Durable fix for all MAME light gun games (generator change, or a global
  default in `default.cfg`)? **TBD**, follow-up change.
- How the Sinden's buttons are mapped to MAME gun buttons by Batocera (which
  physical button is gun button 2) is **TBD** in the specs.
