# input-devices Specification

## Purpose
How input devices map to players and how special devices (the Sinden Light
Gun) are expected to behave. Covers today's expectations; changes to this
behaviour go through `openspec/changes/`.

## Requirements

### Requirement: Arcade kits map to fixed player slots
Each of the four Micro Center arcade control kits MUST map to a fixed player
slot (kit 1 to player 1 through kit 4 to player 4), and that mapping SHALL
remain stable across reboots and when other controllers are attached.

#### Scenario: Normal boot
- **WHEN** Batocera starts with only the four arcade kits attached
- **THEN** each kit controls the same player number it did on the previous boot

#### Scenario: Extra controller attached
- **WHEN** an SNES, N64, or PlayStation-style USB controller is also connected
- **THEN** players 1 to 4 remain assigned to the arcade kits

> Open question: how the extra controllers should be assigned (e.g. take
> player 1 for console systems, or take the next free slot) is TBD and should
> be decided via a change proposal.

### Requirement: Arcade kits navigate EmulationStation
The arcade kits SHALL be configured in EmulationStation so that any of them can
navigate menus and launch games.

#### Scenario: Menu navigation
- **WHEN** a player uses an arcade kit joystick and buttons in EmulationStation
- **THEN** the menu responds with directional movement, select, and back actions

### Requirement: Console-style controllers for matching systems
The SNES, N64, and PlayStation-style USB controllers SHALL be usable in games
for their corresponding systems when attached.

#### Scenario: Playing an N64 game with the N64 controller
- **WHEN** the N64 USB controller is attached and an N64 game is launched
- **THEN** the controller's inputs, including the analog stick and C buttons, work in the game

### Requirement: Sinden Light Gun support
The Sinden Light Gun MUST be used with Batocera's Sinden support enabled, and a
Sinden border MUST be visible on screen during light gun games so the gun can
track the display.

#### Scenario: Launching a light gun game
- **WHEN** a light gun game is launched with the Sinden Light Gun attached
- **THEN** the Sinden driver/software is running and a white border is shown around the game image

#### Scenario: Light gun not attached
- **WHEN** the Sinden Light Gun is not connected
- **THEN** non-light-gun games run normally with no border
