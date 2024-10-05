---
title: Prunt Board 2
type: docs
---

{{% include "images/prunt_board_2_line_art.md" %}}

## Features
- 6× TMC2240 stepper drivers, all capable of running at 3A with minimal airflow.
- 2× 20A heater outputs.
- 4× fan outputs, supporting up to 2A 4-pin fans or 500mA 2-pin fans.
- 4× thermistor inputs supporting PT1000 and most common NTC thermistors.
- 4× endstop inputs.
- 40A maximum input current at 12-24V.
- Fully isolated USB to protect upstream devices.
- Reverse polarity protection.
- Protection against electrical shorts to 24V on most exposed pins.
- Hardware generated step generation at up to 2.54MHz (limited by TMC2240) with 128 velocity levels updated at 20kHz.
- Fail-safe firmware updates, eliminating the need to ever install a jumper.
- Hardware counters for high-speed fan tachometers.
- Soft-start, allowing for a wider range of power supplies.

## Size and Connectors
{{% include "images/prunt_board_2_size.md" %}}

## Schematic
{{< cards >}}
  {{< card link="/prunt_board_2_schematic.pdf" title="Download Schematic" >}}
{{< /cards >}}
