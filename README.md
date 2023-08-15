# candleLight FD

candleLight FD is a modern, CAN-FD capable version of the 
[candleLight](https://github.com/linux-automation/candleLight/) based on a STM32G0B1 microcontroller.

Out of the box it comes with a single CAN channel placed.
But footprints for a second CAN channel are available.

The candleLight FD is - like the candleLight - licensed under the CERN OHL v1.2 open hardware license.

![candleLight FD revision 1](./release/candlelightfd-S01-R01/candleLightfd-S01-R01_3D.jpg)

## Where to get one

The candleLight FD is available from [Linux Automation](https://linux-automation.com)
as a SMD-assembled and functionally tested kit.

## Hardware

### Interfaces

The candleLight FD uses a USB-C (USB 2 only) connector to connect to your PC.

CAN is provided on the industry standard D-SUB9 pinout:

   * CAN1:
     * High: Pin 7
     * Low: Pin 2
   * CAN2:
     * High: Pin 8
     * Low: Pin 1
   * Power:
     * 12V: Pin 9 (can be provided via P1)
     * GND: Pin 3, Pin 6

### Releases: Revision 1

[Manufacturing Data](./release/candlelightfd-S01-R01) |
[Schematic](./release/candlelightfd-S01-R01/candlelightfd-S01-R01-V01/candlelightfd-S01-R01.pdf) |
[Interactive BOM](./release/candlelightfd-S01-R01/candlelightfd-S01-R01-V01/candlelightfd-S01-R01_BOM.html)

(This is the current release.)

Release Notes:

  * Initial release of the candleLight FD.

## Firmware

The firmware is based on the [candleLight_fw](https://github.com/candle-usb/candleLight_fw).
Select the board config `BOARD_candleLightFD` when building the firmware.

> **_NOTE_**: We are currently upstreaming our changes to the *candleLight_fw* repository.
> While doing so you can find our sources 
> [here](https://github.com/linux-automation/candleLight_fw/tree/topic/candleLightFD).