---
title: G-Code Reference
type: docs
---

## Introduction

Prunt g-code uses a simple syntax similar to Marlin. Most commands are also compatible with Marlin commands. The following rules describe the Prunt g-code syntax:

-   Each line may contain zero or one commands.
-   Commands are made up of parameters, which are a single letter from A to Z, and arguments, which may be any of the following:
    -   An integer from 0 to 999.
    -   A floating point number with an optional decimal part (automatically promoted from integer type).
    -   A string surrounded by `"` characters.
-   An argument is only allowed directly after a parameter.
-   Arguments are optional.
-   Duplicate parameters are not allowed.
-   A command must contain a `G` or `M` parameter.
-   Parameters may be in any order.
-   Unused parameters are not allowed.
-   Arguments must be of the correct type for a command.
-   All spaces on a line are ignored, excluding those within strings.
-   Comments starting with `;` outside of a string are ignored.

## G0: Rapid Linear Move

Perform a non-print linear move. Axes which are not specified will not move. Moves at the maximum feedrate if feedrate is not specified.

| Parameter | Type  | Description                                                    |
|-----------|-------|----------------------------------------------------------------|
| [X]       | Float | X axis target position in mm, or offset in relative move mode. |
| [Y]       | Float | Y axis target position in mm, or offset in relative move mode. |
| [Z]       | Float | Z axis target position in mm, or offset in relative move mode. |
| [E]       | Float | E axis target position in mm, or offset in relative move mode. |
| [F]       | Float | Feedrate in mm/min, maximum if not specified.                  |

## G1: Linear Move

Perform a linear move. Axes which are not specified will not move. Moves at the same feedrate as the last G1 command if feedrate is not specified.

| Parameter | Type  | Description                                                    |
|-----------|-------|----------------------------------------------------------------|
| [X]       | Float | X axis target position in mm, or offset in relative move mode. |
| [Y]       | Float | Y axis target position in mm, or offset in relative move mode. |
| [Z]       | Float | Z axis target position in mm, or offset in relative move mode. |
| [E]       | Float | E axis target position in mm, or offset in relative move mode. |
| [F]       | Float | Feedrate in mm/min, from last G1 if not specified.             |

## G4: Dwell

Pause the printer for a given number of seconds after the last move is completed.

| Parameter | Type  | Description               |
|-----------|-------|---------------------------|
| S         | Float | Time to pause in seconds. |

## G10: Retract

Perform a retraction move with the values specified by the last M207 command. Multiple G10 commands without a G11 command between them are ignored.

No parameters.

## G11: Recover

Perform a recovery move with the values specified by the last M207 and M208 commands. Multiple G11 commands without a G10 command between them are ignored.

No parameters.

## G21: Millimetre Units

Does nothing. Provided for compatibility with other motion controllers where G21 sets the units to millimetres.

No parameters.

## G28: Auto Home

Home the specified axes using the method and parameters specified in the configuration. If no axes are specified then all axes are homed, including the E axis.

| Parameter | Type | Description                                |
|-----------|------|--------------------------------------------|
| [X]       | None | If included then the X axis will be homed. |
| [Y]       | None | If included then the Y axis will be homed. |
| [Z]       | None | If included then the Z axis will be homed. |
| [E]       | None | If included then the E axis will be homed. |

## G90: Absolute Positioning

Set the printer to absolute positioning mode. In this mode G0 and G1 specify absolute coordinates. M83 overrides this behaviour for the E axis. This command acts as-if a M82 command is run at the same time, meaning that M83 must be called again to set the extruder to relative mode if it was called previously.

No parameters.

## G91: Relative Positioning

Set the printer to relative positioning mode. In this mode G0 and G1 specify relative coordinates. M82 overrides this behaviour for the E axis. This command acts as-if a M83 command is run at the same time, meaning that M82 must be called again to set the extruder to absolute mode if it was called previously.

No parameters.

## G92: Set Virtual Position

Set the current position to be used by other g-code commands. This does not change how other parts of Prunt see the position, so features such as bounds still function as expected.

| Parameter | Type  | Description                                                                            |
|-----------|-------|----------------------------------------------------------------------------------------|
| [X]       | Float | The X position to set. If not specified then the X axis position will not be adjusted. |
| [Y]       | Float | The X position to set. If not specified then the Y axis position will not be adjusted. |
| [Z]       | Float | The X position to set. If not specified then the Z axis position will not be adjusted. |
| [E]       | Float | The X position to set. If not specified then the E axis position will not be adjusted. |

## M0/M1: Pause

Pause the printer and wait for the user to command the printer to continue.

No parameters.

## M17: Enable Motors

Enable the motors assigned to the specified axes. If no axes are specified then all axes are enabled. On a CoreXY machine specifying X or Y will enable all XY motors.

| Parameter | Type | Description                              |
|-----------|------|------------------------------------------|
| [X]       | None | If included, enable the X axis steppers. |
| [Y]       | None | If included, enable the Y axis steppers. |
| [Z]       | None | If included, enable the Z axis steppers. |
| [E]       | None | If included, enable the E axis steppers. |

## M18/M84: Disable Motors

Enable the motors assigned to the specified axes. If no axes are specified then all axes are disabled. On a CoreXY machine specifying X or Y will disable all XY motors. Disabled axes are marked as unhomed.

| Parameter | Type | Description                               |
|-----------|------|-------------------------------------------|
| [X]       | None | If included, disable the X axis steppers. |
| [Y]       | None | If included, disable the Y axis steppers. |
| [Z]       | None | If included, disable the Z axis steppers. |
| [E]       | None | If included, disable the E axis steppers. |

## M82: E Axis Absolute

Set the E axis to absolute positioning mode, overrides G90. Cleared by G90.

No parameters.

## M83: E Axis Relative

Set the E axis to relative positioning mode, overrides G91. Cleared by G91.

No parameters.

## M104: Set Hotend Temperature

Set the target temperature for the hotend.

| Parameter | Type  | Description                    |
|-----------|-------|--------------------------------|
| S         | Float | Target temperature in celcius. |

## M106: Set Fan Speed

Set the speed for a given fan. If no fan is specified then the first fan is used.

| Parameter | Type           | Description                                                             |
|-----------|----------------|-------------------------------------------------------------------------|
| [P]       | Integer/String | Fan name or index. First fan if not specified.                          |
| [S]       | Float          | Speed from 0 to 255 with 255 being maximum speed. 255 if not specified. |

## M107: Fan Off

Turn off the specified fan. If no fan is specified then the first fan is used.

| Parameter | Type           | Description                                    |
|-----------|----------------|------------------------------------------------|
| [P]       | Integer/String | Fan name or index. First fan if not specified. |

## M109: Wait for Hotend Temperature

Wait until the hotend reaches or exceeds a given temperature.

| Parameter | Type  | Description                         |
|-----------|-------|-------------------------------------|
| S         | Float | Temperature to wait for in celcius. |

## M122: TMC Register Dump

Log all register values for all connected Trinamic stepper drivers.

No Parameters.

## M140: Set Bed Temperature

Set the target temperature for the bed.

| Parameter | Type  | Description                    |
|-----------|-------|--------------------------------|
| S         | Float | Target temperature in celcius. |

## M141: Set Chamber Temperature

Set the target temperature for the chamber.

| Parameter | Type  | Description                    |
|-----------|-------|--------------------------------|
| S         | Float | Target temperature in celcius. |

## M190: Wait for Bed Temperature

Wait until the bed reaches or exceeds a given temperature.

| Parameter | Type  | Description                         |
|-----------|-------|-------------------------------------|
| S         | Float | Temperature to wait for in celcius. |

## M191: Wait for Chamber Temperature

Wait until the chamber reaches or exceeds a given temperature.

| Parameter | Type  | Description                         |
|-----------|-------|-------------------------------------|
| S         | Float | Temperature to wait for in celcius. |

## M205: Set Dynamic Kinematic Limits


| Parameter | Type  | Description                                                                          |
|-----------|-------|--------------------------------------------------------------------------------------|
| P         | None  | Required to prevent accidental usage of commands meant for other motion controllers. |
| [A]       | Float | Acceleration in mm/s². Not modified if not specified.                                |
| [J]       | Float | Jerk in mm/s³. Not modified if not specified.                                        |
| [S]       | Float | Snap in mm/s⁴. Not modified if not specified.                                        |
| [C]       | Float | Crackle in mm/s⁵. Not modified if not specified.                                     |
| [D]       | Float | Path deviation in mm. Not modified if not specified.                                 |
| [L]       | Float | Pressure advance time in s. Not modified if not specified.                           |

## M207: Retraction Settings

Adjust the retraction settings used by G10 and G11.

| Parameter | Type  | Description                                                |
|-----------|-------|------------------------------------------------------------|
| [F]       | Float | Feedrate in mm/min. Not modified if not specified.         |
| [E]       | Float | E axis retraction distance. Not modified if not specified. |
| [Z]       | Float | Z axis retraction distance. Not modified if not specified. |

## M208: Recovery Settings

Adjust the recovery settings used by G11.

| Parameter | Type  | Description                                                         |
|-----------|-------|---------------------------------------------------------------------|
| [F]       | Float | Additional feedrate in mm/min. Not modified if not specified.       |
| [E]       | Float | Additional E axis recovery distance. Not modified if not specified. |

## M486: Ignored

Ignored command provided for slicer compatibility.

| Parameter | Type  | Description |
|-----------|-------|-------------|
| [S]       | Float | Ignored.    |
| [T]       | Float | Ignored.    |
