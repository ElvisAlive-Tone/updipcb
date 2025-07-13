How to use SerialUDPI in PlatformIO based project using `pymcuprog`
=================================================================

Source: https://docs.platformio.org/en/latest/platforms/atmelmegaavr.html#upload-using-pymcuprog-serialupdi

`pymcuprog` is provided by Microchip company and is [open source](https://github.com/microchip-pic-avr-tools/pymcuprog).

Important files, which has to be in the [PlatformIO](https://platformio.org/) project's root folder are:
* `platformio.ini`

How to use
----------

Open the `platformio.ini` and edit any MCU related settings as usually for a new project. Do not forget to change `--device` in the `[env:pymcuprog_upload]` section!

On top of that:
* make sure python 3 is installed
* install `pymcuprog` using `python -m pip install pymcuprog`
* make sure `pymcuprog` is on `PATH` (see potential warnings from its installation)
* make sure USB UART dongle driver is installed
* COM port of that USB UART dongle used by UPDI programmer is autodetected
* set the upload speed. With the CH330 based programmer i can easily achieve 961000 baud. The max value depends on the USB UART dongle type and the UPDI implementation. 115200 should always work.

Then use common `PlatformIO:Upload` command.

How to use `pymcuprog`
---------------------

[pymcuprog](https://github.com/microchip-pic-avr-tools/pymcuprog/blob/main/help.md) commandline can be also used for some tasks like:

`pymcuprog -d attiny402 -t uart -u COM6 erase`

This command erases program from the MCU. 
You have to adjust device name and COM port!