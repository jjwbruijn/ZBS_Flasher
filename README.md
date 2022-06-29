# ZBS_Flasher

### By Aaron Christophel <https://ATCnetz.de>

### You can support my work via PayPal: <https://paypal.me/hoverboard1> this keeps projects like this coming

Ardunio / C++ Library and interface to flash the ZBS243 / SEM9110 8051 Microcontroller

The ZBS243 is an 8051 Zigbee Enabled SOC with 64KB of Flash and 1KB of EEPROM Like Infopage.

It is used in the Electronic Shelf Labels made by SOLUM and rebranded with the name SEM9110

<img width="600" alt="ZBS243 Picture" src="pictures/zbs243_example.jpg">
<img width="600" alt="ZBS243 Pinout" src="pictures/zbs243_pinout.jpg">

The debug interface is an SPI like connection and the debug mode is entered via toggling the CLK signal while a reset is done.

Many more infos about the ZBS243 can be found here: [Reverse Engineering an Unknown SOC](https://dmitry.gr/?r=05.Projects&proj=30.%20Reverse%20Engineering%20an%20Unknown%20Microcontroller)

This Project enables simple Reading and Writing of the Flash/Infopage

It is written for the ESP32 but should run basically on any SOC with ~8 GPIOS and UART.

# You need

  Virsual Studio code
  
  PlatformIO
  
  Python > 3.0
  
    for Python you need: pyserial ( pip install pyserial )
  
# How to get Started

  Load the Project folder into Virsual Studio Code, select the Correct COM port in the platformio.ini
  
  Flash it to an ESP32.
  
  After that you can use the python tool zbs_flasher.py(zbs_flasher.exe precompiled) with the following parameter:
  
    zbs_flasher COMxx read/write/readI/writeI file.bin <- file from where to read or write to
  
  The Pinout on how to connect the ZBS243 SOC is in this README or readable in the main.cpp file and can be changed to any wanted GPIOs of the ESP32.
  
  The "ZBS_POWER" pin is used to reset the SOC after any debug mode action like flashing etc. the SOC is stuck in debug mode
  
  and can only exit from it via a power cycle so it is adived to connect the power to the SOC via an.
  
  transistor to automaticaly have it exit the debugmode and run the firmware.


# Pins:
ZBS243 Pin                       |Pin name                       |Name                       |ESP32 Pin
:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
14 | P0_0 | SPI_Clk | 18
15 | P0_1 | SPI_MoSi |  5
16 | P0_2 | SPI_MiSo |  17
17 | P0_3 | SPI_SS |  23
22 | RST | RESET |  19
ALL VCC | VCC | ZBS243 Power | 16

# General infos

- Before each flashing the Flash/Infopage is always fully erased
- Flash takes ~130 Seconds for a full file, smaller file are faster of course
- Full read of flash takes ~70 Seconds
- on a Flash the data is directly verified on the ESP32, on reading no verify is done so maybe read it twice to make sure its correct
- It could be needed to tweak the transmission speed from the ESP32 to ZBS, this can be done in the zbs_interface.h (ZBS_spi_delay value)
