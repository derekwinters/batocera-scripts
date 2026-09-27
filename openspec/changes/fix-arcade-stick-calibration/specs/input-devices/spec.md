## ADDED Requirements

### Requirement: Arcade kit joysticks rest at centre after boot
Each arcade kit joystick MUST rest at centre (no direction held) after any
boot, whether a reboot or a cold power-on. Moving a kit joystick before any
other input on that kit MUST NOT leave a direction held. The SDL calibration
cache for the arcade kit encoders SHALL record every axis as centred, and the
arcade SHALL restore that cache at boot if it is missing or wrong, without
changing the cache files of other controllers.

#### Scenario: Cold boot with the joystick used first
- **WHEN** the arcade is powered on from off and a player pushes a kit joystick left, then right, before touching any other input on that kit
- **THEN** each push registers as its own direction and, when released, the joystick returns to centre with no direction held

#### Scenario: Reboot with the joystick used first
- **WHEN** Batocera reboots and a player moves a kit joystick before any other input on that kit
- **THEN** the movement registers correctly and no direction stays held after the joystick is released

#### Scenario: Calibration cache missing
- **WHEN** the arcade kit encoder calibration cache under `/userdata/system/.sdl2/` has been deleted
- **THEN** at the next boot it is restored with all four axes centred

#### Scenario: Calibration cache wrong
- **WHEN** the arcade kit encoder calibration cache records any axis at a value other than centre (for example, the minimum `-32768`)
- **THEN** at the next boot it is rewritten with all four axes centred, and the cache files of other controllers are left unchanged
