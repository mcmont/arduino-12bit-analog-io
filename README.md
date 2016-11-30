# Arduino Uno 12-bit analog I/O

## Overview
This Arduino Uno firmware implements 8 12-bit analog input and output channels. The Arduino controls a 12-bit National Semiconductor (now Texas Instruments) [ADC128S052](http://www.ti.com/lit/ds/symlink/adc128s052-q1.pdf "View datasheet") analog to digital converter (ADC) IC and an Analog Devices [AD5628](http://www.analog.com/media/en/technical-documentation/data-sheets/AD5628_5648_5668.pdf "View datasheet") digital to analog converter (DAC) IC using the [SPI bus](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface_Bus).

This firmware uses event-based execution driven by serial interrupts. When the user issues a command the relevant IC is selected by setting its slave select (SS) pin LOW, a command is sent and (in the case of the ADC) the response is read.

## IC pinout
The IC pins are designated by their names, rather than by their pin numbers. This is because different packages are available and pin layouts vary between them. 

| Arduino pin 	| ADC128S052 	| AD5628  |
|-------------	|-----------	|-------- |
| 8  SS        	| CS   	      | -      	|
| 9  SS        	| -    	      | SYNC   	|
| 10 +5V out [1]  	    | -    	      | VDD    	|
| 11 MOSI	    | DIN  	      | DIN    	|
| 12 MISO	    | DOUT [2]    | -      	|
| 13 SCLK       | SCLK 	      | SCLK   	|

[1] This digital pin provides sufficient current to power the DAC IC. The +5V supply trace should be decoupled by placing a 10uF tantalum bead capacitor in parallel with a 0.1uF ceramic capacitor to ground within 1cm of the AD5628 VDD power pin.

[2] Via a 100 Ohm pull-up resistor to +5V.

## Other IC connections and ancilliary components
### ADC128S052
| Pin name | Connect to |
|-------------	|-----------	|
| AGND  | Analog GND     |
| DGND  | Digital GND [3]|
| VA    | +5V [4]        |
| VD    | +5V [5]        |

[3] When creating a board design you should create analog and digital ground planes that should be electrically connected in one place only.

[4] VA is the positive analog supply pin. This voltage is also used as the reference voltage. This pin must Power 2 VA be connected to a quiet 2.7-V to 5.25-V source and bypassed to GND with 1-µF and 0.1-µF monolithic ceramic capacitors located within 1 cm of the power pin.

[5] VD should be connected to a quiet +2.7V to +5.25V source and bypassed to GND with 1μF and 100nF monolithic ceramic capacitors  located within 1 cm of the power pin.

You may wish to add an op-amp input stage to limit the voltages that can be applied to the ADC128S052 input channel pins.

### AD5628
| Pin name | Connect to |
|-------------	|-----------	|
| GND | GND (obvz)|
| LDAC | GND |
| CLR | GND |
| REFIN| +5V |

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
