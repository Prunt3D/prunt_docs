---
title: Command-line Arguments
type: docs
---

## Prunt
The following command-line arguments are accepted by a standard Prunt server:

| Argument                             | Default | Description                                                                                                                                                                 |
|--------------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `--prunt-gui-port=`                  | 8080    | Port to bind the web GUI to.                                                                                                                                                |
| `--prunt-gui-host=`                  |         | Host to bind the web GUI to. Default allows connections from anywhere.                                                                                                      |
| `--prunt-max-planner-block-corners=` | 50000   | Maximum number of corners that can be planned as a single block. Around 1.5kB is used per corner, but this may change in future releases.                                   |
| `--prunt-motion-planner-cpu=`        | 0       | CPU to assign the motion planner task to. Default of 0 does not assign the task to a specific CPU. Generally this should be left at 0.                                      |
| `--prunt-step-generator-cpu=`        | 0       | CPU to assign the motion planner task to. Default of 0 does not assign the task to a specific CPU. Generally this should be set to an isolated CPU to maximise reliability. |

## Prunt Board 2
The following additional arguments are accepted by Prunt Board 2:

| Argument                | Default | Description                                                                                                                                                                 |
|-------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `--serial-port=`        |         | The serial port path of the board. Must be specified. This should usually be `/dev/serial/by-id/usb-Prunt_3D_Prunt_Board_2_…` where … depends on your specific board.       |
| `--communications-cpu=` | 0       | CPU to assign the motion planner task to. Default of 0 does not assign the task to a specific CPU. Generally this should be set to an isolated CPU to maximise reliability. |
