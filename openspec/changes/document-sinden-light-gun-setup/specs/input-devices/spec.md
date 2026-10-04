## MODIFIED Requirements

### Requirement: Sinden Light Gun support
The Sinden Light Gun MUST be used with Batocera's Sinden support enabled. A
Sinden border MUST be visible on screen during light gun games so the gun can
track the display. The border SHALL be shown only in gun games, which are
listed per game with `<system>["<rom>"].bordersmode=normal` on top of a global
`controllers.guns.bordersmode=hidden`. Non-gun games MUST NOT show the border,
even while the Sinden is attached. The Sinden calibration SHALL be stored on
the gun itself and MUST persist across reboots.

#### Scenario: Launching a light gun game
- **WHEN** a light gun game listed with `bordersmode=normal` is launched with the Sinden Light Gun attached
- **THEN** the Sinden driver/software is running and a white border is shown around the game image

#### Scenario: Gun attached, non-gun game launched
- **WHEN** the Sinden Light Gun is attached and a game that is not listed with `bordersmode=normal` is launched
- **THEN** the game runs with no border

#### Scenario: Light gun not attached
- **WHEN** the Sinden Light Gun is not connected
- **THEN** non-light-gun games run normally with no border

#### Scenario: Calibration survives a reboot
- **WHEN** the Sinden has been calibrated and the arcade is rebooted
- **THEN** the gun aims at the same points as before the reboot, without recalibrating
