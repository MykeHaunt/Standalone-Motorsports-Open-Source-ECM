<div align="center">

<b>GPL open-source ECU</b>

[![Release](https://img.shields.io/github/v/release/FOME-Tech/fome-fw?style=flat)](https://github.com/FOME-Tech/fome-fw/releases/latest) ![Last Commit](https://img.shields.io/github/last-commit/FOME-Tech/fome-fw?style=flat)
[![Unit Tests](https://img.shields.io/github/actions/workflow/status/FOME-Tech/fome-fw/build-unit-tests.yaml?label=Unit%20Tests&branch=master)](https://github.com/FOME-Tech/fome-fw/actions/workflows/build-unit-tests.yaml)
![GitHub commits since latest release (by date)](https://img.shields.io/github/commits-since/FOME-Tech/fome-fw/latest?color=blueviolet&label=Commits%20Since%20Release)
[![FOME Discord](https://img.shields.io/discord/1060875162892898324?label=Discord&logo=Discord)](https://discord.gg/EEg2fbhQD4)

</div>

# [Free Open Motorsports ECU](https://www.fome.tech/)

# <span style="color:#1E90FF">Standalone Motorsports Open Source ECM</span>

The <span style="color:#1E90FF">Standalone Motorsports Open Source ECM</span> is a fully open-source engine control module platform designed for high-performance racing engines. Derived from the <span style="color:#8A2BE2">FOME</span> project, it provides OEM-level control and customization.

## <span style="color:#228B22">Key Features</span>

- <span style="color:#FF8C00"><b>Fuel Injection</b></span>: Supports sequential and batch injection (up to 12 injectors), 3D VE tables, enrichment, decel fuel cut, and closed-loop AFR using wideband O₂ sensors.
- <span style="color:#DC143C"><b>Ignition Control</b></span>: Wasted spark and fully sequential up to 12 cylinders. Includes base, cranking, and idle timing maps, multi-spark, boost retard, dwell settings, and launch control.
- <span style="color:#20B2AA"><b>Sensors</b></span>: Interfaces for crank/cam (VR/Hall), MAP, TPS, CLT, IAT, oil/fuel pressure, flex-fuel, knock sensors, and more. ~10 analog inputs plus thermistor channels.
- <span style="color:#FFD700"><b>Outputs</b></span>: Controls injectors, ignition coils, ETB, fans, pumps, and relays via low/high-side drivers and PWM outputs.
- <span style="color:#8B008B"><b>Communication</b></span>: CAN (1 Mbit/s), USB (virtual COM), UART, SPI/I²C. Data logging via SD card, live telemetry, and integration with TunerStudio and FOME Console.
- <span style="color:#4682B4"><b>Configuration</b></span>: Real-time tuning, sensor calibration, multi-dimensional maps, closed-loop strategies, and Lua scripting engine for custom algorithms.
- <span style="color:#B22222"><b>Safety</b></span>: Includes rev/boost limiters, fuel/oil pressure shutdown, fault handling with LEDs and limp modes, and debug streaming.

## <span style="color:#228B22">System Architecture</span>

- <span style="color:#1E90FF"><b>Firmware</b></span>: Runs on STM32F4/F7 ARM Cortex-M with a deterministic main loop. Sensor acquisition, fuel/spark calculations, and actuator commands occur every control cycle.
- <span style="color:#DC143C"><b>Timing</b></span>: Precise injector/spark timing via hardware timers synchronized to crank position. Lower-priority tasks like logging run asynchronously.
- <span style="color:#8A2BE2"><b>Extensibility</b></span>: Lua scripts can manipulate sensor/actuator values in each onTick cycle.

## <span style="color:#228B22">Hardware Requirements</span>

- <span style="color:#1E90FF"><b>MCU</b></span>: STM32F4/F7 with CAN, USB/UART, high-speed ADCs, timers, and sufficient I/O.
- <span style="color:#20B2AA"><b>Inputs</b></span>: VR/Hall crank & cam, analog sensors (MAP, TPS, IAT, CLT), digital switches (clutch, AC, neutral), flex-fuel, wheel speed, O₂ sensors.
- <span style="color:#FFD700"><b>Outputs</b></span>: Injector drivers, ignition coils, ETB PWM, fan/pump relays, logic outputs, CAN stream.
- <span style="color:#8B008B"><b>Expansion</b></span>: SPI/I²C for additional sensors or modules.

## <span style="color:#228B22">Setup & Tuning</span>

- <span style="color:#4682B4"><b>Build</b></span>: Uses <span style="color:#8A2BE2">gcc-arm-none-eabi</span>. Build via Make or STM32CubeIDE. Flash with FOME Console, STM32CubeProgrammer, or dfu-util.
- <span style="color:#4682B4"><b>Tuning</b></span>: Configure fuel and ignition maps, calibrate sensors, tune PID for idle and ETB, set limiters and enable closed-loop AFR. Use TunerStudio/FOME Console.

## <span style="color:#228B22">Operation</span>

- <span style="color:#1E90FF"><b>Start-Up</b></span>: Bench test → install → crank → tune. Monitor AFR, temp, RPM, and knock.
- <span style="color:#FFD700"><b>Live Tuning</b></span>: On-the-fly edits to tables. Real-time data via CAN or USB. Logs to SD or external logger.
- <span style="color:#B22222"><b>Diagnostics</b></span>: LEDs indicate faults. Debug Mode exposes internal states. Fault codes shown in software.
- <span style="color:#B22222"><b>Safety</b></span>: CRITICAL LED lights on major faults. ECU cuts fuel/spark on sync loss, overboost, etc.

## <span style="color:#228B22">Development & Contribution</span>

- <span style="color:#4682B4"><b>Toolchain</b></span>: gcc-arm-none-eabi, Java JDK, Make, CMake. Use GitHub for contributions.
- <span style="color:#4682B4"><b>Standards</b></span>: Modular code, real-time safe. Write unit tests and documentation. All code under GPLv3+.

## <span style="color:#228B22">Credits</span>

- <span style="color:#8A2BE2"><b>Based On</b></span>: FOME and rusEFI
- <span style="color:#8A2BE2"><b>Maintainer</b></span>: MykeHaunt
- <span style="color:#8A2BE2"><b>License</b></span>: GPLv3+

## <span style="color:#228B22">Diagrams</span>

### <span style="color:#1E90FF">System Architecture Diagram</span>

```plaintext
+----------------------------+
|        Engine Sensors      |
| (MAP, TPS, CLT, Crank, Cam)|
+-------------+--------------+
              |
              v
     +------------------+
     |   ECM Firmware    |
     |  (STM32F4/F7 MCU) |
     +--------+---------+
              |
     +--------+---------+
     |  Fuel & Ignition  |
     |    Controllers     |
     +--------+---------+
              |
   +----------+-----------+
   | Injector | Coil Ctrl |
   | Drivers  | (Spark)   |
   +----------+-----------+
              |
+-------------+-------------+
|      Engine Actuators      |
|  (Injectors, Coils, ETB)   |
+----------------------------+
```

### <span style="color:#1E90FF">Tuning Workflow</span>

```plaintext
+------------------+
|   Flash Firmware  |
+--------+---------+
         |
         v
+--------+---------+
|  Connect ECU via  |
|  FOME Console/TS  |
+--------+---------+
         |
         v
+--------+---------+
| Calibrate Sensors |
+--------+---------+
         |
         v
+--------+---------+
| Set Fuel/Ign Maps |
+--------+---------+
         |
         v
+--------+---------+
| Tune on Dyno/Road |
+------------------+
```


# Release Notes

See [the changelog](firmware/CHANGELOG.md), or [by release](https://github.com/FOME-Tech/fome-fw/releases).
