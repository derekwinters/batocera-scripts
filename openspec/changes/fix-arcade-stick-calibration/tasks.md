## 1. Device files

- [x] 1.1 Add the known-good calibration cache at `userdata/system/.sdl2/03000000be3200000020000011010000_Baolian industry Co., Ltd TS-UAIB-OP02.cache` containing exactly `4\n-129\n-129\n-129\n-129\n`
- [x] 1.2 Add the user service `userdata/system/services/arcade_stick_calibration` (POSIX sh, executable, idempotent, `set -u`-safe) that restores the cache on `start` when missing or different, with the known-good content built in, `stop` as a no-op, and `status` reporting whether the cache matches
- [x] 1.3 Check the script with `sh -n` (and shellcheck if available) and check the cache file bytes with `od -c`
- [x] 1.4 Test the service locally against a temp directory: missing file, bad file (`-32768` x4), already-correct file (no rewrite), other cache files untouched

## 2. On-device deployment

- [ ] 2.1 Copy both files to the same paths under `/userdata` on the arcade and make the service executable (`chmod +x`)
- [ ] 2.2 Enable the service: `batocera-services enable arcade_stick_calibration` (or System Settings > Services)
- [ ] 2.3 Confirm the cache on the device reads `4` then `-129` four times, and that other files in `/userdata/system/.sdl2/` are unchanged

## 3. On-device verification

- [ ] 3.1 Cold boot (power off, then on) and push a kit joystick first, before any other input on that kit; confirm left and right register correctly and nothing stays held
- [ ] 3.2 Repeat the cold-boot check a second time (at least two cold boots with no stuck direction)
- [ ] 3.3 Reboot twice, pushing a kit joystick first each time; confirm no stuck direction on either reboot
- [ ] 3.4 Delete the cache on the device, reboot, and confirm the service restored it with centred values

## 4. Wrap-up

- [ ] 4.1 Archive the change (`/opsx:archive`) so the spec deltas merge into `openspec/specs/`, and close issue #2
