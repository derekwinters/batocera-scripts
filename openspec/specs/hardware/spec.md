# hardware Specification

## Purpose
Inventory of the physical arcade setup as it exists today: the host running
Batocera, the display, and the input devices that are always or occasionally
attached. Unknown details are marked TBD.

## Requirements

### Requirement: Host machine
The arcade SHALL run Batocera Linux on a Minisforum mini PC (exact model TBD).

#### Scenario: System boots into the arcade
- **WHEN** the mini PC is powered on
- **THEN** it boots Batocera Linux and lands in EmulationStation

#### Scenario: Persistent configuration
- **WHEN** settings, scripts, ROMs, or saves are changed
- **THEN** they are stored under `/userdata` on the host so they survive Batocera upgrades

### Requirement: Display
The arcade SHALL output to a single 27-inch 4K monitor.

#### Scenario: Video output
- **WHEN** EmulationStation or a game is running
- **THEN** video is shown on the 27-inch 4K monitor

### Requirement: Permanently attached arcade controls
The arcade SHALL have four Micro Center universal arcade control kits (joystick
plus buttons, each with its own USB encoder) permanently connected to the host,
one per player position 1 through 4.

#### Scenario: Arcade controls present at boot
- **WHEN** Batocera starts
- **THEN** four USB arcade encoders are detected as game controllers

### Requirement: Occasionally attached USB controllers
The arcade SHALL support the following additional USB devices being plugged in
as needed, alongside the four arcade kits:
- Super Nintendo (SNES) style USB controller (model TBD)
- Nintendo 64 style USB controller (model TBD)
- PlayStation / PlayStation 2 style USB controller (model TBD)
- Sinden Light Gun (model/count TBD)

#### Scenario: Hot-plugging an extra controller
- **WHEN** one of the additional USB controllers is plugged in
- **THEN** Batocera detects it as an input device without requiring changes to the arcade kit setup

#### Scenario: Extra controller removed
- **WHEN** an additional controller is unplugged
- **THEN** the four arcade kits continue to work as players 1 to 4
