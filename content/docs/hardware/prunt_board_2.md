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

## Size and Connector Positions
{{% include "images/prunt_board_2_size.md" %}}

## Connectors
### USB
- The USB port connects to and powers a 6Mb/s USB-UART adaptor with an isolated connection to the rest of the board.

### Power
- The power supply screw terminals support solid or stranded wires from 6-20AWG / 0.5-10mm² and must be torqued to 1.2Nm.
- The board contains reverse polarity protection to prevent against accidental miswiring; however, applying AC or quickly switching between polarities will destroy the board.
- The board may be powered from a 12-24V power supply.
- Maximum current must be limited to 40A with an external components if the power supply is capable of continuously supplying over 40A.
- The board may not power on if the power supply voltage is over 27V or under 8.3V. The 27V cutoff does not imply that a power supply voltage over 24V is supported.
- The board contains a soft-start circuit so no large load will be applied to the power supply at power-on.

### Steppers
- The numbers above the stepper ports correspond to the numbers in the GUI.
- Note that the stepper pinout is different from some other boards, specifically each stepper phase should be connected to two adjacent pins.

### Switch Inputs
- Each input pin is pulled high by a 4.7kΩ resistor with a series diode.
- Input pins are protected from overvoltage inputs.
- The 4-6V pin will only rise above approximately 5V in the event of an overvoltage event on the board (including some events not on the switch input pins).
- Overvoltage on a 4-6V pin will cause all other 4-6V pins to rise to the applied voltage but will not cause damage to the board itself.
- Power and ground pins are otherwise protected, pay careful attention to the pinout to ensure that the power pins are not shorted to ground.
- Total current draw must be limited to 500mA.
- Minimum negative-going threshold voltage: 1.4V
- Maximum positive-going threshold voltage: 3.4V
- Hysteresis: 0.5-1.1V

### ADC Inputs
- One side of each ADC input is pulled to 3.3-3.6V via a 2kΩ resistor.
- Applying a large current to the ground side of any ADC input (e.g. shorting to 24V) may blow a replaceable fuse, disconnecting ADC ground from board ground. This will not damage the board but ADC inputs will be non-functional until the fuse is replaced.
- An overvoltage event on the other side of a given ADC input will not impact other ADC pins.

### Fans
- Each fan power pin is protected by a replaceable fuse (2A by default).
- Ground pins are not protected.
- PWM pins are protected by a smart low-side load switch.
- PWM pins are capable of driving 500mA each and may be used to directly drive a 2 pin fan.
- PWM pins may operate from 1Hz to 50kHz.
- The PWM pins operate in a slow decay mode with a diode to 24V when driving inductive loads.
- 3 pin fans are not supported unless they are not driven by the PWM pin or the tachometer pin is left disconnected.
- The board contains jumper pins for pulling each PWM pin to 4-6V for obscure fans that may require this.
- Each tachometer pin is pulled high by a 4.7kΩ resistor with a series diode.
- Tachometer minimum negative-going threshold voltage: 1.4V
- Tachometer maximum positive-going threshold voltage: 3.4V
- Tachometer hysteresis: 0.5-1.1V

### Heaters
- The heater screw terminals support solid or stranded wires from 12-26AWG / 0.2-4mm² and must be torqued to 0.5Nm.
- Each heater power pin is protected by a replaceable fuse (10A and 5A by default).
- Switched ground pins are not protected.
- Heaters may operate from 1Hz to 100Hz.
- Heaters may switch up to 20A.

## Flashing
### Main MCU
{{< callout type="warning" >}}
  We do not prohibit the use of third-party firmware; however, due to the fact that we can not audit it, the user assumes full responsibility for any hardware damage that may occur as a result of installing or using third-party firmware.
{{< /callout >}}

The main MCU can be automatically updated by the Prunt server and should never need to be flashed manually while running Prunt, however it is still possible to do so by installing one of the included jumpers on the pins marked as BOOT0.

### Secondary MCU
{{< callout type="warning" >}}
  We do not prohibit the use of third-party firmware; however, due to the fact that we can not audit it, the user assumes full responsibility for any hardware damage that may occur as a result of installing or using third-party firmware.
  
  In addition, any damage caused while attempting to flash the secondary MCU is also the responsibility of the user as the secondary MCU is not designed to be flashed by the user.
{{< /callout >}}

The secondary MCU on the USB side acts as a USB-UART bridge and should never need to be updated however it is possible to do so.

When the MCU is placed in to system memory boot mode the USB connector becomes a UART connector, therefore an adaptor is required.
Some early units may include an adaptor.
If building an adaptor, connect the TX pin (input pin on STM32) to D+ and the RX pin (output pin on STM32) to D-.

{{< callout type="warning" >}}
  Do not place the board in to system memory boot mode while connected to a normal USB port as this may damage the devices on both ends.
{{< /callout >}}

To flash the MCU:
- Disconnect all power and unplug the USB port.
- Plug a USB cable which is disconnected at both ends in to the Prunt board.
- While shorting the two pads circled in the below image with a sharp pair of tweezers, plug the other end of the USB cable in to a powered adaptor.
- Use stm32flash or your preferred tool to flash the new firmware.

<img src="/images/prunt_board_2_usb_mcu_boot0.jpg" width="100%">

## Klipper
{{< callout type="warning" >}}
  The user assumes full responsibility for any damage caused by any bugs in Klipper or misconfiguration of Klipper.
{{< /callout >}}

A custom version of Klipper is currently required on this board as upstream Klipper does not support the following features:
- USART3 on PC11/PC10 on STM32G474 (PR awaiting merge).
- HSE clock bypass mode (PR awaiting merge).
- Soft start support (no PR as the code is specific to this board).

A fork supporting these features is available from the git repository located at https://github.com/liampwll/klipper.git in the `prunt_board_2_support` branch.

The following settings must be set in Klipper's `menuconfig`:

{{% include "images/prunt_board_2_klipper_menuconfig.md" %}}

Additionally the baud rate may be set to a value up to 6,000,000.
Note that the onboard USB-UART adaptor will switch to 6 MBaud if a baud rate of 75 is set on the host side.

{{< callout type="warning" >}}
  Do not attempt to bypass the soft start circuitry by setting a pin state at startup or manually setting the pin high from Klipper as this is likely to damage the board.
{{< /callout >}}

## Schematic
{{< cards >}}
  {{< card link="/prunt_board_2_schematic.pdf" title="Download Schematic" >}}
{{< /cards >}}
