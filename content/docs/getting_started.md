---
title: Getting Started
type: docs
---

## Basic Setup
Below are the approximate steps for setting up a printer with a Prunt board:

1. Connect the board to the various motors/thermistors/etc., noting the labels on the board where each is connected.
2. Power on the board and run the server binary on the host machine (for Prunt Board 2 it can be downloaded from [https://github.com/Prunt3D/prunt_board_2_server/releases](https://github.com/Prunt3D/prunt_board_2_server/releases)). Note that a command line argument specifying a serial port is required for some boards (`--serial-port=/dev/serial/by-id/usb-Prunt_3D_Prunt_Board_2_…` for Prunt Board 2, where … is replaced with the path for your board).
3. Navigate to <host_address>:8080 in a web browser.
4. Navigate to "Config Editor" at the top of the page.
5. Go through each tab and configure the board as desired using the built-in descriptions. Note that the save button only saves the current page.
6. Finally navigate to the Prunt tab in the config editor and tick the box to enable Prunt and save.
7. Restart the server binary.

It is also heavily recommended to set up CPU isolation for a single CPU which is then passed to the [`--prunt-step-generator-cpu=` command line argument](/docs/command_line_arguments/).
For Prunt Board 2 an additional CPU should be isolated and passed to `--communications-cpu=`.

## CPU Isolation

CPU isolation prevents any tasks from running on a given CPU if they are not assigned to that specific CPU.
This is useful for Prunt as it can prevent other tasks from taking away resources from the time-sensitive step generator task.

Example of parameters to isolate the third and fourth CPU: `nohz=on nohz_full=2,3 rcu_nocbs=2,3 isolcpus=nohz,domain,managed_irq,2,3 rcu_nocb_poll`

To add these parameters on OpenSUSE or SUSE: Run `yast2`, navigate to `boot loader > kernel parameters`, and append the parameters to the existing parameters.

To add these parameters on Raspberry Pi OS: Edit `/boot/cmdline.txt` and append the parameters to the existing parameters.

Note that CPUs in Prunt start from 1 whereas CPUs in Linux start from 0, therefore for the above parameters 3 or 4 should be passed to Prunt rather than 2 or 3.
