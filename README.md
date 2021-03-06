Realtek RTD2660/2662 programmer
===============================

##Firmware programmer for LCD controllers based on the RTD2660 or RTD2662 chips.

The Realtek RTD2662 chip sometimes mislabeled as RTD2660 is found in many cheap LCD controller boards, most popular seems to be the PCB800099. In order to support different LCD panel resolutions, one has to load the "correct" firmware on the board.

If you search around the net you can find many different firmware images for this controller. I found that loading the firmware into the board was not for the weak of heart and requires some shady tools from eBay.

Here is a project how to build a programming tool for less than $15.

To build the programmer you would need a basic FX2LP device. One from amazon or ebay based on CY7C68013A would do. Install the FX2LP SDK from Cypress and use the Cypress Control Center tool to flash the i2c.iic file from the USB-I2C folder on the FX2LP device.

Many other hardware devices can be used to program the LCD controller - the protocol is based on the i2c standard.

Connect:
```
    FX2LP       VGA Port
    Device      Target
    ------      ---------------
    PA0    ---> Pin 15 DDC SCL
    PA1    ---> Pin 12 DDC SDA
    GND    ---> GND (choose any of the white ports, not the VGA connector itself)
```
You would need to power the target board from a 12V or 5V power supply.

Use the PC software to flash a .bin file to the target or to read the content of the existing flash. Saving a copy of the existing software is recommended.

The code may require some modification if your LCD controller is using different SPI flash chip to store the firmware. I've only tested this code with Winbond SPI flash chips.

Building the software:

There is a Windows PC project for visual studio 2010. Multiplatform support is pending.

The FX2LP device software needs the Keil PK51 toolchain.  It is a very simple firmware that implements i2c protocol and can be commanded via USB. There is a compiled binary i2c.iic you can upload to the FX2LP device as well.

##Python fork of the programmer

Was created to use native i2c bus, available on raspberry pi. For the moment it is added python realization which is identical to original programmer, written in C++. You need just 3 wires attached to raspberry pi

Connect:
```
    Raspberry   VGA Port
    Pi          Target
    ------      ---------------
    J8-3    ---> Pin 15 DDC SCL (or pin 11 of white row connector)
    J8-5    ---> Pin 12 DDC SDA (or pin 12 of white row connector)
    J8-6    ---> GND (choose any of the white ports, not the VGA connector itself) (or pin 10 of white row connector)
```

You need to enable i2c interface in raspi-config (if not yet) and then reboot.
Also you need to install smbus module for python (since programmer uses smbus to talk over i2c).

##Adding Macronix 0xC22013 chip to the supported chips and manufactures in pyprog.py

Usage of this script is as follows:
python pyprog.py -r original.bin
python pyprog.py -w new.bin
Binaries can be taken from https://github.com/raparram/Programador-I2C-RTD2660-Rpi3/tree/master/PCB800099

Steps I'm using to upload new firmware:
1) Setup raspberry and connection as described in https://github.com/raparram/Programador-I2C-RTD2660-Rpi3
2) Download needed firmware to raspberry from https://github.com/raparram/Programador-I2C-RTD2660-Rpi3/tree/master/PCB800099
3) Save existing flash by running: python pyprog.py -r original.bin
4) Flash with needed firmware by running: python pyprog.py -w new.bin
5) Connect everything together and check the display works correctly.
