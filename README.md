# arduino-12bit-analog-io


## Overview
Implements 8 x 12-bit analog inputs and 8 x 12-bit analog outputs using an Arduino Uno.

This Arduino Uno firmware is used to control a 12-bit ADC128S052 analog to 
digital converter (ADC) IC and an AD5628 digital to analog converter (DAC) 
IC.

This firmware uses event-based execution driven by serial interrupts. When the user issues a command the relevant IC is selected, a command is sent and (in the case of the ADC) the response is read.

## Serial communications
To communicate with the Arduino use a serial over USB serial connection at 115200 baud, 8 data bits, 1 stop bit, no parity, no flow control. When connecting to the board you can optionally set the DTR line to LOW for 50ms to force the Arduino to do a hardware reset.

To read the voltages of the 8 analog inputs send an ASCII CR character over the serial connection. The values are returned as a string of zero-padded 4-digit numbers to provide a constant return string length. 

To change an analog output channel's voltage send a string with the channel number (1 based; 1-8), an ASCII space character, then the 12-bit value (0-4095) that corresponds to the desired voltage. For example, if the AD5628 voltage reference pin is set to 5V, sending the command:

1 2047<CR>

will change the output voltage of output channel 1 to 2.5V.

You can change the voltage output of all of the channels simultaneously by 
selecting channel zero, e.g:

0 0<CR>

sets all output channels to 0V.

Strings are returned by the Arduino prefixed with MESG if the information is a message to the user, or with DATA if the following string should be interpreted as analog measurement data.

## Credits
Dr. Chris Empson, School of Chemistry, University of Leeds 2015.
https://github.com/mcmont/arduino-12bit-analog-io


## License
All code is made available under the Creative Commons Attribution (CC-BY) license v4.0.
[https://creativecommons.org/licenses/by/4.0/](https://creativecommons.org/licenses/by/4.0/ "View license")
