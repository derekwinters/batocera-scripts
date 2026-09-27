## Why

After a boot, an arcade kit joystick can get stuck holding one direction (for
example, pushing left or right both register as "right" held forever, and
wiggling the stick does not clear it). It happens intermittently, when a kit
joystick is the first input used on that kit after boot (issue #2).

The cause is a bad axis-calibration cache. The kits' USB encoders
(TS-UAIB-OP02, USB ID `32be:2000`) do not send an initial HID report, so until
an input changes, the kernel reports every axis at its minimum (0). Batocera
generated its SDL axis-neutral cache for these encoders from those untouched
values, so it records the neutral position as `-32768` (full deflection)
instead of centre (`-129`). Deleting the cache does not help: Batocera
regenerates the same wrong values, and it only writes the file when it is
missing.

## What Changes

- Ship a known-good SDL calibration cache for the arcade kit encoders at
  `/userdata/system/.sdl2/03000000be3200000020000011010000_Baolian industry Co., Ltd TS-UAIB-OP02.cache`
  that has all four axes centred (`-129`).
- Add a Batocera user service, `arcade_stick_calibration`, in
  `/userdata/system/services/`. At boot it checks that cache file and rewrites
  it with the known-good content if it is missing or different. The known-good
  content is built into the script, so the service works even if it is the
  only file deployed. It changes no other controller's cache file.
- Enable the service on the device with
  `batocera-services enable arcade_stick_calibration` (or in
  System Settings > Services).

## Capabilities

### New Capabilities
<!-- none -->

### Modified Capabilities
- `input-devices`: new requirement that arcade kit joysticks rest at centre
  after any boot, and that using a joystick first does not leave a direction
  held.
- `hardware`: record the arcade kit encoder facts (model, USB ID, SDL GUID,
  axis layout and range, no initial report) in the "Permanently attached
  arcade controls" requirement.

## Impact

- New device files under `userdata/system/.sdl2/` and
  `userdata/system/services/` (deployed to the same paths under `/userdata`).
- The cache files for other controllers are not touched.
- No change to player mapping, EmulationStation input config, or emulator
  config.
