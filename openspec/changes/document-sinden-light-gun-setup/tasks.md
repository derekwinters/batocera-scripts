## 1. Config

- [x] 1.1 Capture the gun-related `batocera.conf` lines (border, crosshair, Time Crisis) in `userdata/system/batocera-lightgun.conf` with comments
- [x] 1.2 On the arcade: set `controllers.guns.bordersmode=hidden`, add `bordersmode=normal` for each gun game, and remove the system-wide `mame.use_guns=1`

## 2. On-device verification

- [ ] 2.1 Owner calibrates the Sinden and confirms the aim is accurate at the four corners and the center of the screen
- [ ] 2.2 Owner reboots the arcade and confirms the aim is unchanged

## 3. Wrap-up

- [ ] 3.1 Archive the change (`/opsx:archive`) so the spec delta merges into `openspec/specs/`, and close issue #6
