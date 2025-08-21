# eChook BMS Comunications Master Board
This project is heavily inspired by the [SimpBMS](https://github.com/Tom-evnut/SimpBMS) project by [Tom-evnut](https://github.com/Tom-evnut). The SimpBMS Hardware was never open-sourced, and has become obsolete post-COVID with the demise of the Teensy 3.x development boards.  
This board uses a Teensy 4.1 to mirror, then expand on the functionality of the original SimpBMS board.

While effort has been made to maintain some cross compatibility with the SimpBMS in connector choices and pinouts, this board is **not** a drop in replacement.

## License
This hardware design is released under a non-commercial open hardware licence, is provided **"as is"**, without warranty of any kind, express or implied. See the License.md file for details.

## Features

**ALL UNTESTED**

In addition to an expanded feature set over the SimpBMS, this board offers far more comprehensive circuit protection, most notably offering electrically isolated power and comms for the Battery Module connections, short circuit protection on all off board voltage feeds, so a harness issue does not take down the BMS, and 24v overvoltage protection on all digital and analogue inputs.

IO Overview:  
- 3 CAN Busses (All ESD protected, one Isolated for Battery Module comms).
    - Switchable Terminating Resistor for each bus.
- 2 UART, ESD protected and buffered.
    - One Isolated for Tesla Battery Module Comms.
    - One non-isolated, with selectable voltage levels.
- 1 RS-458/Modbus Interface
- 4 24v safe digital inputs
- 4 Monitored and Overcurrent protected 12v 5A High Side Drivers
- 4 5A Low Side Drivers
- 3 Short Circuit protected 12v outputs
- 4 Short Circuit Protected 5V outputs (One Isolated)
- 2 Analogue Inputs for Current Sensor signals, 24V safe.
- 1 I2C header for 0.91" OLED display.
- 1 SD card slot on the Teensy 4.1 to allow for logging with RTC timestamping.
- Ultra Bright Red error LED
- Warning Buzzer indicating BMS error or blown fuse (On, unless actively turned off by Teensy)


A little more detail:  
- Networks:
    - Electronically isolated communication busses for battery slave module communications.
        - Isolated 5V UART port for Tesla Module Communications (Teensy Serial2)
        - Isolated 600mA 5V supply 
        - Isolated CAN (Teensy CAN1) Connection for (most other) battery modules that communicate over CAN
    - Two non-isolated CAN busses, one exposed on the RJ-45 connector (CAN3), a second exposed on the J5 Auxillary Comms connector (CAN2)
    - One RS-485/MODBUS interface (Teensy Serial8)
    - RJ-45 connector with VICTRON compatible pin out as default, but 'Pick Your Own Pinout' pads on the bottom of the board to allow this to be modified to any application with a bit of soldering.
    - Serial output on J5 (Serial7). Voltage is selectable between 5V and 3.3v by solder jumper on rear of board.
- I/O
    - 4 High Impedence digital inputs, up to 24V.
    - 4 12V High Side Driver outputs, each with voltage, current and temperature monitoring, automatic over temperature, short circuit protection and error state reporting.
    - 4 PWM Capable Low Side Driver outputs with over current/short circuit protection, over temperature protecton. LOW Speed PWM only - testing needed, suspect <2KHz.
- Voltage Supply
    - 5V 500mA Isolated voltage supply for Tesla BMS Boards. Short circuit protected with red fault LED
    - 3 5V 500mA Voltage supplies on J2, J4 and J5, each short circuit protected with red fault LED
    - 3 12v Supplies on J4, Pins 10 and 11 sharing one short circuit protection ciruit, Pin 9 with it's own short circuit protection circuit.
- Warning System
    - Audible warning via a buzzer. This sounds if the onboard fuse blows, or if it is not actively being turned off by the Teensy.
    - Visual warning via 10mA Red LED. This is driven by the Teensy.
    - Heartbeat - Green LED located in cheesy silkscreen image of a heart will blink regularly if the system is healthy.

## Firmware
Due to the swap to a Teensy 4.1, expanded capabilities of the board, and a few pinout changes that go along with both of those, this board is not directly compatible with the SimpBMS code. There is an updated fork of the TeslaBMSV2 firmware adapted for this board [here](https://github.com/GryphonR/TeslaBMSV2). 

## Tesla module harnessing

Tesla Module BMS Connecor part number appears to be: 
Molex Mini-Fit TPA, 10ckt 15-97-5101 
https://www.molex.com/en-us/products/part-detail/15975101

Mouser Links:

15975101 MINIFIT TPA 10CKT Receptical Housing: https://www.mouser.co.uk/ProductDetail/Molex/15-97-5101  
TPA Key: https://www.mouser.co.uk/ProductDetail/Molex/15-97-9101  
CPA Key: https://www.mouser.co.uk/ProductDetail/538-15-97-0071  
Pins: https://www.mouser.co.uk/ProductDetail/Molex/39-00-0038
