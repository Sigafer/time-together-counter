# Time-Together Counter

A pocket-watch-sized PCB that shows time passed, from a past moment, in the format `YY:DDD:HH:MM:SS`. The 1.3" OLED (SH1106 128×64 I²C) stays dark until someone double-taps the device. It runs standalone from a small LiPo charged over USB-C every few months.

Designed in Altium Designer by Zsigmond Csaba.

## How it works

A low-power STM32 sleeps until the accelerometer detects a double-tap in hardware. It then reads the real-time clock, computes the time elapsed since a fixed start timestamp, and shows it on the OLED for about 5 seconds before going back to sleep. The RTC keeps counting the whole time, so the count survives resets and never drifts more than a few seconds per year.

| Block | Part | Notes |
| --- | --- | --- |
| MCU | STM32L031K6U6 | ~1 µA in Stop mode, programmed over SWD (TC2030) |
| Real-time clock | Micro Crystal RV-3028-C7 | 1 Hz tick on CLKOUT, very low drift |
| Wake on double-tap | ST LIS2DW12 | Tap detection in hardware, INT1 wakes the MCU |
| Display | 1.3" 128×64 SH1106 I²C OLED module | Always on 3V3, put to sleep between taps |
| Charger | Microchip MCP73831 (4.2 V) | 100 mA charge current, status read by the MCU |
| Regulator | TI TPS7A0233 (3.3 V) | 25 nA quiescent current |
| Battery | 502030 protected LiPo, ~250 mAh | JST SH 2-pin connector |
| Charging port | GCT USB4125-GF-A | USB-C, power only (5.6k CC pull-downs) |


### I²C addresses

| Device | Address |
| --- | --- |
| RV-3028-C7 (RTC) | 0x52 |
| LIS2DW12 (accelerometer) | 0x18 |

## Repository layout

```
hardware/altium/            Altium project: .PrjPcb and .SchDoc
hardware/altium/libraries/  component libraries used by the project (.SchLib, .PcbLib)
hardware/production/        Gerbers, BOM and pick-and-place files (later)
firmware/                   STM32 firmware (later)
docs/                       schematic PDF and other documentation
artwork/                    line art for the black art side of the board (later)
mechanical/                 3D-printed back cover (later)
```

## License

   Copyright 2026 Csaba Zsigmond

   This hardware design is licensed under the CERN Open Hardware Licence
   Version 2 – Permissive (CERN-OHL-P v2). See [LICENSE](LICENSE).
