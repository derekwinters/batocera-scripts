## ADDED Requirements

### Requirement: MAME light gun aim comes only from the Sinden
In MAME light gun games, the light gun's aim MUST come only from the Sinden
Light Gun. No mouse or joystick axis (including an arcade kit joystick) SHALL
be added to the gun's X or Y axis, so the aim does not depend on the state of
any other input device.

#### Scenario: Aiming in Time Crisis without touching the arcade kits
- **WHEN** Time Crisis is running and no arcade kit joystick has been touched since boot
- **THEN** the crosshair lines up with where the Sinden points, across the whole screen

#### Scenario: Arcade kit joystick moved during a light gun game
- **WHEN** a player moves an arcade kit joystick while Time Crisis is running
- **THEN** the gun's crosshair does not move

### Requirement: Time Crisis is playable with the Sinden Light Gun
Time Crisis (`timecris`) SHALL run on Batocera's standalone MAME and be
playable with the Sinden Light Gun. The ROM set, including the `namcoc71`
device ROM, SHALL verify as good.

#### Scenario: Launching Time Crisis
- **WHEN** Time Crisis is launched from EmulationStation
- **THEN** it starts on standalone MAME (not the MAME 2003-Plus libretro core) and reaches gameplay

#### Scenario: ROM set check
- **WHEN** `/usr/bin/mame/mame -rompath /userdata/roms/mame -verifyroms timecris` is run on the arcade
- **THEN** it reports "romset timecris is good"

#### Scenario: Shooting
- **WHEN** the player aims the Sinden at the screen and pulls the trigger
- **THEN** the game fires at the crosshair position (trigger is gun button 1)

#### Scenario: Leaving cover
- **WHEN** the player holds gun button 2 (the game's foot pedal)
- **THEN** the character leaves cover, and releasing it returns to cover

> Note: Time Crisis II, 3 and 4 are not playable in MAME. They are flagged
> `MACHINE_NOT_WORKING` in MAME itself (`timecrs2`, `timecrs3`, `timecrs4`).
> PS2 ports are the likely future route for II and 3 (TBD); 4 is PS3-only and
> out of scope.
