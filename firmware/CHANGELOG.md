<!---

How to use this file:
- When making a change to the firmware that users should know about, add it to the
	appropriate section (breaking/added/fixed) in the next release.
- If it's something interesting or breaking, make a post about it on the forum! We want users
	to find out about changes before they have to get help because their car won't start.

Release template (copy/paste this for new release):

## Month 202x Release - "Release Name"
or
## Unreleased

### Breaking Changes
 - 

### Added
 -

### Fixed
 - 

### Removed
 - 

 -->

# Changelog

## Unreleased

### Added
 - Allow fractional tachometer pulse ratio for fine tachometer calibration
 - Add an option to ramp the idle target down as engine speed returns to idle. Makes the running -> idle transition much smoother while in closed loop mode #570
 - Lua functions `storePersistentValue`/`getPersistentValue` to persist values in backup memory between reboots #592
 - Fully disable VVT when the target is 0. This prevents trying to control VVT when near the stop, which can damage the locking pin on some engines.

## February 2025 Release

### Breaking Changes
 - Large data logging improvements and changes - in TunerStudio, you may need to re-create your dash layouts from default and then re-add any broken gauges and indicators.

### Added
 - Read (some) OBD2 error codes using a normal scan tool! Currently limited to some common sensor errors. (#529 thank you @dynfer for implementation, @paleppp for testing, and others!)
 - Support Miata NA CAS "CMP" signal as a cam input (if you've installed a 36-1 crank trigger on an NA, for example) #495
 - Support 4 independent wideband O2 channels, and 4 fuel trim banks
 - Flash error codes for some sensors using the check engine light #526
 - Implement knock sensing support for STM32H7 chips
 - Show knock sensing window on FOME console engine sniffer

### Fixed
 - Idle VE table's indicator dot properly tracks when using an idle VE Y-axis override
 - Minor performance improvements to injection/ignition scheduling
 - Per-cylinder knock sensing while in wasted spark mode (fixed by #553)
 - Improved transitions between fuel/spark modes, improves cranking and startup (#551, #553. #554)
 - Disable radiator fans while ignition switch is off

## September 2024 Release

### Added
 - Staged injection: a second set of fuel injectors that inject part of the fuel load when you run out of primary injector
 - VE/Ign/Boost blend tables can select a Y axis override for even more flexibility
 - Added Lua function `getAuxDigital` for parity with rusEFI #351
 - Optional table to change after start idle taper time #369 (thank you, @Krakert!)
 - Increased precision available for Lua tables
 - Add EGT values to CAN broadcast format #398
 - Add options to enable/disable optional CAN frames (cams, EGT so far)
 - Add 1-5-4-8-3-7-2-6 and 1-6-5-10-2-7-3-8-4-9 firing orders
 - Console command `set_sensor_mock` now accepts a sensor by name, instead of index. Example: `set_sensor_mock CLT 85.5`
 - TunerStudio UI improvements (#436, etc)
 - Dropdown selector for popular gearbox ratios (#358, thank you @alrijleh and @nmschulte!)
 - Add two more aux linear sensors #476
 - Support wasted spark on odd cylinder count and odd-fire engines. Improves startup and allows running without a cam sensor!
 - Add an option for the DFCO MAP threshold to use a table dependent upon RPM #485 (thank you @alrijleh!)
 - Option to disable DFCO on gear shift #487
 - Ability to use an 8x8 table for after-start fuel multiplier that depends on CLT and engine run time
 - Start-up tachometer sweep!

### Fixed
 - Improve performance with Lua CAN reception of a high volume of frames
 - Displayed units in TunerStudio change when switching between volume vs. mass injector flow modes #42
 - Make Toyota "3 Tooth Cam" decoder more robust #382
 - Flex sensor-derived fuel temperature indication works properly
 - Fix a scenario where noisy trigger can cause overdwell [rusefi/rusefi#6349](https://github.com/rusefi/rusefi/issues/6349)
 - Fix decel fuel cutoff (DFCO) on engines without a MAP sensor #457
 - Automatic calculation of knock sense frequency based on cylinder bore #379
 - Removed MAP sampling option that can result in inaccurate MAP sensor readings
 - Improve TunerStudio performance on STM32F4-based ECUs #474
 - Inhibit VVT control if the cam position sensor dies
 - Fix "Alpha-N IAT density correction" option

## December 2023 Release

### Breaking Changes
 - Simplified VVT single tooth cam modes. "Single tooth second half" and "single tooth first half" have been replaced by simply "single tooth". This may require re-setting your trigger and VVT timing offsets if you used "single tooth first half" prior to this change. #178

### Added
 - Log per-cylinder true ignition timing (includes trim, knock retard, etc) #76
 - Add mode for CLT/IAT sensors that are installed "high side" instead of typical "low side" #116
 - Automatic supply voltage compensation for VVT solenoids, giving more stable control as battery voltage changes #209
 - Configurable ignition timing limits, adding safety particularly for engines running wasted spark with a large cam #211
 - Improved injection, ignition bench test UI and functionality: More gauges, fuel pump on/off buttons for injector testing, and configurable duration/count #219
 - Preset for Mazdaspeed Miata (NB) IAT sensor
 - Option to compensate Alpha-N fueling with intake air temperature
 - Gasoline-scale AFR gauge: instead of showing true AFR, show "gasoline scale" where 14.7 means stoichiometric
 - Decode fuel temperature from flex fuel sensors #254
 - Allow brief operation over 100% injector duty cycle and add configurable duty cycle limits #215
 - Buttons to bump VVT targets for testing/PID tuning
 - Improved serial port selection for FOME console and firmware update
 - Better default configuration values for some advanced features
 - Support BMW E8x/E9x CAN speed sensor input (works for standalone MK60e5 too)

### Fixed
 - Improved bench test resolution (more usable for testing injectors, dwell, etc)
 - Maximum knock retard table displays correct Y axis values in TunerStudio
 - Make errors about fuel pressure sensors less aggressive #111 #117
 - Always operate in "two wire" mode for batch fuel, fixing batch firing order #23
 - Fix "Toyota 3 Tooth Cam" VVT mode (1JZ, 2JZ, 1G, etc) actually works now #237
 - Fix autotune while TS project is in AFR mode - corrections now made properly for target AFR table values
 - Changing idle stepper settings causes kernel panic #265
 - Data log channel "Fuel: Injection timing SOI" indicates correct values

## May 2023 Release

### Added
 - Detected gear and wastegate position to CAN format
 - Y axis override for Idle VE table. Helps with idle quality on ITB setups. #94
 - Blend tables for boost open and closed loop control #73
 - Log channels to make MegaLogViewer VE Analyze work #82
 - Enable knock sensing on F407 Discovery build, hardcoded to PA3 #105
 - Support gauge and differential fuel pressure sensors (in addition to absolute) #32
 - Snapshot builds have a date code in the folder name #91

### Fixed
 - Sensor checker wasn't checking sensors
 - Removed vestigial warning about unset ignition pins #79
 - Gear detection sometimes didn't work #86
 - Restore built-in INI storage on STM32F7-based ECUs
 - Improve log field names, easier to find the channel you're looking for #98

### For older changelog entries prior to fork from rusEFI
see [CHANGELOG.rusefi.md](CHANGELOG.rusefi.md)
