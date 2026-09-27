## MODIFIED Requirements

### Requirement: Permanently attached arcade controls
The arcade SHALL have four Micro Center universal arcade control kits (joystick
plus buttons, each with its own USB encoder) permanently connected to the host,
one per player position 1 through 4.

Each kit's USB encoder is a "Baolian industry Co., Ltd TS-UAIB-OP02"
(USB ID `32be:2000`, SDL GUID `03000000be3200000020000011010000`). Each encoder
exposes 4 absolute axes (ABS_X, ABS_Y, ABS_Z, ABS_RZ) with range 0 to 255 and
rest/centre at 127, 13 buttons, and 1 hat. The digital joystick is reported on
the axes. The encoder sends no initial report, so until an input on it
changes, every axis reads 0 (the minimum).

#### Scenario: Arcade controls present at boot
- **WHEN** Batocera starts
- **THEN** four USB arcade encoders are detected as game controllers

#### Scenario: Encoder identity
- **WHEN** the arcade kit encoders are listed on the host (e.g. with `lsusb`)
- **THEN** each appears as USB ID `32be:2000`, "Baolian industry Co., Ltd TS-UAIB-OP02", with SDL GUID `03000000be3200000020000011010000`

#### Scenario: Axes before first input
- **WHEN** Batocera has just started and no input on a kit has changed yet
- **THEN** that kit's four axes read 0, and after the joystick has been moved and released they read 127
