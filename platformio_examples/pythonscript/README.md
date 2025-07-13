How to use SerialUDPI in PlatformIO based project
=================================================

Source: https://github.com/SpenceKonde/megaTinyCore/discussions/606

Important files, which has to be in the PlatformIO project's root folder are:
* `platformio.init`
* `updi_ipload.py`

How to use
----------

Place both files in the root directory of the project, where platformio.ini normally resides.
Open the `platformio.ini` and edit any MCU related settings as usually for a new project.

Ontop of that:
* make sure USB UART dongle driver is installed
* determine the COM port of the USB UART dongle used as updi programmer and update the corresponding setting, ie. `upload_port = /dev/ttyUSB2` or `upload_port = COM3`
* set the upload speed. With the CH330 based programmer i can easily achieve 961000 baud. The max value depends on the usb-uart dongle type and the updi implementation. 115200 should always work.
* change the monitor port to the same value if using the same uart for tracing.

Then use common `PlatformIO:Upload` command.

To set the fuses invoke:

`pio run -t fuses -e set_fuses`


How it works
------------

The important part is to replace the built in stock programming method with a custom one. This is done in the ini file by setting the

`upload_protocol = custom`

and providing an extra python script:

`extra_scripts = updi_upload.py`

The script does the following:

* grabs the serial port settings and the MCU type from the platformio.ini file
* finds the path to the pio's python path (pyenv)
* finds the path to the `prog.py`, which is installed together with the core files
* using the above invokes the prog.py with imported settings using the correct python path, basically automating the manual way presented in the readme file here
    https://github.com/SpenceKonde/megaTinyCore/blob/master/megaavr/extras/PlatformIO.md#using-serialupdi-from-platformio

Fuses
-----

Programming fuses is also implemented. The fuse values are calculated when using the command:

`pio run -t fuses -e set_fuses`

The returned values are formatted for avrdude, the scripts reformats them into `offset:value` pairs accepted by the prog.py. Programming the lockbits is not implemented, because the avrdude format does not provide the offset value.

