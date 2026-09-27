## Context

The four arcade kits use "Baolian industry Co., Ltd TS-UAIB-OP02" USB encoders
(USB ID `32be:2000`, SDL GUID `03000000be3200000020000011010000`). Each exposes
4 absolute axes (ABS_X, ABS_Y, ABS_Z, ABS_RZ), range 0..255 with centre/rest at
127, plus 13 buttons and 1 hat. The digital joystick is reported on the axes.

Batocera caches each controller's detected axis neutral values in
`/userdata/system/.sdl2/<GUID>_<name>.cache`. The format is: first line is the
axis count, then one line per axis with the SDL-scaled neutral value.

Findings on the device (2026-09-27):

- The encoder sends no initial HID report. Until an input changes, the kernel
  reports every axis as 0 (the minimum). Untouched kits read 0 on all four
  axes. A kit that has been moved reads 127.
- The cache for these encoders contains `4` followed by `-32768` four times
  (the minimum), because it was generated from the untouched 0 values.
- Deleting the file and restarting EmulationStation regenerates the same bad
  values. The bad file was dated December 2025 and had not changed since, so
  Batocera appears to write it only when it is missing.
- With neutral recorded as full deflection, a joystick pushed before the
  encoder has reported centre can leave a direction held that wiggling does
  not clear.
- Raw 127 on a 0..255 axis scales to `-129` in SDL. Other controllers' caches
  on the device show `-129`/`128` for centred axes.

## Goals / Non-Goals

**Goals:**
- Each arcade kit joystick rests at centre after every reboot and cold boot,
  whatever input is used first.
- The fix survives Batocera upgrades (lives under `/userdata`) and repairs
  itself if the cache is deleted or regenerated wrongly.

**Non-Goals:**
- Changing other controllers' cache files.
- Changing player mapping, EmulationStation input mapping, or emulator config.
- Fixing the encoder firmware's missing initial report.

## Decisions

### Ship a static cache file with the correct neutral values

The correct content is fixed and known: `4\n-129\n-129\n-129\n-129\n`.
Because Batocera only writes the cache when it is missing, putting this file
in place is enough by itself to fix the problem, and Batocera will keep using
it.

Alternatives considered:
- *Delete the cache and let Batocera regenerate it.* Rejected: it regenerates
  the same bad values because the encoders still read 0 at detection time.
- *Nudge the encoder at boot so it reports centre before detection.* Rejected:
  needs a way to make the device send a report, which is more complex and
  less reliable than writing a known file.

### Guard with a boot-time user service

A Batocera user service, `/userdata/system/services/arcade_stick_calibration`,
runs at boot with `start`. It compares the cache file with the known-good
content and, only if the file is missing or different, writes the content to a
temp file in the same directory and moves it into place (atomic replace). It
leaves every other file in `/userdata/system/.sdl2/` alone. `stop` does
nothing. `status` reports whether the cache matches.

The known-good content is built into the script so that deploying only the
service is enough. The script is POSIX `sh`/busybox-safe, safe to run more than
once, and safe under `set -u`. It logs through `logger` when available,
otherwise to stdout.

User services are enabled with `batocera-services enable arcade_stick_calibration`
or from System Settings > Services in EmulationStation.

## Risks / Trade-offs

- [The service may run after EmulationStation has already opened the
  joysticks, so a repair would only take effect on the next boot] → The
  shipped file already fixes the problem, since Batocera does not overwrite an
  existing cache. The service is a guard for the case where the file is lost
  or regenerated wrongly.
- [A future Batocera version could change the cache format or naming, or
  start rewriting it] → The on-device verification tasks will catch a
  regression. The service would re-apply the known values on each boot, which
  could then be wrong for a new format; revisit if Batocera changes this.
- [Other encoder units could report a different device name, so the file name
  would not match] → All four kits currently report the same name and GUID.

## Open Questions

- Is the user service guaranteed to run before EmulationStation opens the
  joysticks? **TBD.** This does not block the fix, because the shipped cache
  file is what fixes it; the service only guards against the file being lost.
- Exactly when and by which Batocera component the cache is written (only when
  missing is inferred from the file date, not confirmed in source). **TBD.**
