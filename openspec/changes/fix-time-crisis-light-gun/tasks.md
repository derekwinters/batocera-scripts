## 1. Device files

- [x] 1.1 Add `userdata/system/configs/mame/timecris.cfg` binding `:OPT.0` (`P1_LIGHTGUN_X`, mask 65535, default 381) to `GUNCODE_1_XAXIS` and `:OPT.1` (`P1_LIGHTGUN_Y`, mask 65535, default 163) to `GUNCODE_1_YAXIS`
- [x] 1.2 Check the cfg is well-formed XML

## 2. On-device deployment

- [x] 2.1 Copy `timecris.cfg` to `/userdata/system/configs/mame/timecris.cfg` on the arcade
- [x] 2.2 Set `timecris` to run on standalone MAME: `mame["timecris.zip"].emulator=mame` and `mame["timecris.zip"].core=mame` in `/userdata/system/batocera.conf` (or EmulationStation > game > Advanced game options > Emulator)
- [x] 2.3 Add `namcoc71.zip` (`c71.bin`) to `/userdata/roms/mame`
- [x] 2.4 Check the set with `/usr/bin/mame/mame -rompath /userdata/roms/mame -verifyroms timecris` and confirm "romset timecris is good"

## 3. On-device verification

- [x] 3.1 Launch Time Crisis with the Sinden attached and, without touching any arcade kit joystick, confirm the crosshair lines up with where the gun points across the whole screen
- [ ] 3.2 Confirm the trigger fires (gun button 1) and holding gun button 2 (foot pedal) leaves cover
- [ ] 3.3 Exit and relaunch the game; confirm `timecris.cfg` still binds the gun axes to the gun only and the aim still lines up

## 4. Wrap-up

- [ ] 4.1 Archive the change (`/opsx:archive`) so the spec deltas merge into `openspec/specs/`, and close issue #5
