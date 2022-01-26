# Monster FDC - Assembly Instructions

## Prerequisites

### Tools, Equipment, and Supplies

* Soldering iron with a fine tip. Temperature controlled soldering station is recommended
* Small side cutters for cutting components' leads
* Universal programmer capable of programming SST39F010A Flash ROM ICs. For example, MiniPro TL866CS or MiniPro TL866A (optional, Flash ROM can be programmed in-system)
* Multimeter with frequency measurement, an oscilloscope, or a logic analyzer can be beneficial for troubleshooting
* Desk lamp, magnifying glass
* Solder suitable for soldering electronics. For example: rosin core Sn63/Pb37, Sn60/Pb40, or a lead-free solder such as Sn96.5/Ag3.0/Cu0.5 (sometimes referred to as SAC305)
* Solder wick for removing excess of solder
* 99% Isopropyl Alcohol for removing the excess of flux after soldering
* Lint free wipes, used toothbrush, cotton swabs for cleaning the PCB before and after soldering

### Parts

The up to date list of parts provided in the [Bill of Materials](README.md#bill-of-materials) section of the [README.md](README.md) file.
It also provides the recommended sources for the parts.
Additionally, refer to the [Devices and Functionality](README.md#devices-and-functionality)
and [Possible Component Relacement](README.md#possible-component-replacements) sections for alternative and optional components.

Prior to assembling the board, you might want to program the Flash ROM with [Multi-Floppy BIOS extension](https://github.com/skiselev/floppy_bios).
The [floppy_bios.bin](https://github.com/skiselev/floppy_bios/blob/master/floppy_bios.bin) file is the compiled binary ROM image.
Alternatively, you can flash the Flash ROM in system using [xiflash](https://github.com/skiselev/xiflash) utility.

## Assembly Steps

### 1. Gather supplies and parts

* Check that you have all the equipment and parts listed in the [Prerequisites](#prerequisites) section above
* Organize your workspace. If available, use ESD-safe surface and ESD strap when working on this project

### 2. Solder the components

Solder the components going from lower profile components to higher profile components, from smaller components to larger. Here is the recommended order:

* Solder RN1 - RN4 resistor arrays
  * Make sure that resistor arrays are oriented correctly. The first pin of the resistor array is marked with a printed dot, and the first pad on the board has a square shape
* Solder C1 - C2 4.7 nF capacitors
  * These capacitors are optional, and only required for Intel 82077AA FDCs (with any numeric suffix). No need to solder them if using National Semiconductor PC8744 or Intel 82077SL FDCs
    * Note: The board will function correctly if these capacitors are soldered regardless of FDC type
  * These are non-polarized ceramic capacitors, so they can be oriented either way
  * Trim the leads using cutters
* Solder C3 - C17 100 nF capacitors
  * These are non-polarized ceramic capacitors, so they can be oriented either way
  * Trim the leads using cutters
* Solder C18 - C23 10 uF capacitors
  * These are polarized capacitors and must be oriented correctly. The negative (-) side usually marked with paint, and the negative (-) lead is shorted than the positive (+) one
  * Trim the leads using cutters
* Solder X1 - X2 oscillator sockets
  * Make sure that sockets are oriented correctly. The notch on the socket should match with the notch on the board's silkscreen
  * The oscillators can be soldered directly to the board without sockets. This will reduce cost, improve reliability, but make it more difficult to repair the board
* Solder U4 - U10 DIP sockets
  * Make sure that sockets are oriented correctly. The notch on the socket should match with the notch on the board's silkscreen
  * It is recommended to solder two opposite pins first (e.g., pin 1 and pin 9 for a DIP-16 socket), and then verify that the socket is oriented correctly and sits flush on the board. Resolder if needed.
  * Either U5 (DIP-32) socket or U6 (DIP-28) socket should be installed. The choice depends on whether Flash ROM or EEPROM will be used with the board.
  * U4, and U7 - U10 ICs can be soldered directly to the board without sockets. This will reduce cost, improve reliability, but make it more difficult to repair the board
  * It is recommended that U5 or U6 will be socketed, so, if needed, they can be easily extracted for reprogramming
* Solder U1 - U3 PLCC sockets
  * Make sure that sockets are oriented correctly. The cut corner on the socket should match the cut corner on the board's silkscreen
* Solder JP1 - JP9 headers
  * It is recommended to solder one pin first, verify that the header is oriented upright and sits flush with the board, and then solder the rest of the pins
  * Headers JP6 - JP9 should be only installed if the board will be used with IBM PS/2 floppy drives that are powered by pin 3 on the floppy drive connector
* Solder J1 - J4 connectors
  * These connectors need to be oriented correctly. The cutout in the middle of the connector shroud should point down toward the ISA connector
* Finally, solder the J5 connector

### 3. Check your soldering work

* Make sure all pins of all components are soldered properly
* If desired, clean the flux using isopropyl alcohol, cotton swabs. You might want to scrub the board lightly with a used toothbrush to remove the flux

### 4. Insert the integrated circuits to the sockets

* Prior to inserting DIP integrated circuits to the sockets board, bend their leads slightly, so they point 90 degrees downward. Put the IC on the side and gently push it down to bend the leads. Repeat on the other side of the IC
* Double check that you're placing the integrated circuit in the right socket, check the IC orientation. The index notch on the IC should match the notch on the socket and the drawing on the PCB's silkscreen
* To insert the FDC and UART integrated circuits in PLCC packages, place the integrated circuit on the top of the socket, double check the orientation of the integrated circuit, and firmly push the package down. It should click into the socket
* Insert X1 and X2 oscillators into the sockets
* Double check that all ICs and oscillators are oriented correctly:
  * Orient the board with the ISA connector pointing down or toward you
  * U1 - U4 ICs (all PLCC and GD75232 IC in DIP-20 package) should have the print/marking on the packages read bottom to top
  * U5 - U10 ICs and X1, X2 oscillators should have the print/marking on the packages read left to right
  * Note that the print/marking on C18 - C23 capacitors will appear upside down

### 5. Place jumpers on JP1 - JP5 jumper blocks

Refer to the [User Manual](User_Manual.md) for the jumpers' description

**Important**: Do not place JP6-JP9 jumpers unless you're absolutely sure you are using IBM PS/2 floppy drives that are powered by pin 3 of the floppy connector

### 6. Test the Monster FDC controller card

* Plug the card into an ISA slot of an IBM PC/XT or IBM AT compatible computer
* Connect floppy drives' cables to the card's J1 - J4 connectors
* If desired, connect a serial device to the serial port J5 connector. It is possible to use a loopback plug, or a serial mouse to test the serial port
* Power on the system
  * Assuming that the Flash ROM / EEPROM is installed on the card, and it is programmed with the Multi-Floppy BIOS extension,
you should see Multi-Floppy BIOS configuration print-out during the POST (Power-on self test)
  * Once the POST post is complete, you should also see a Multi-Floppy BIOS extension configuration utility prompt. Press **F2** to enter the configuration utility
  * Configure the card according to the drive types you have in your system. Refer to the [User Manual](User_Manual.md) for the configuration utility usage information
* Boot into the OS using either a bootable floppy disk, or HDD/SSD
* Test that FDCs and floppy drives are working by copying files from one floppy drive to another, or from HDD to the floppy drive and vice versa
* Test that the serial port is working using either a diagnostic utility that supports a serial loopback, such as CheckIt, or if using a serial mouse, load mouse driver and make sure the mouse is working

If the Flash ROM was not programmed prior to installing the controller card in the computer, it can be programmed in system, using [xiflash](https://github.com/skiselev/xiflash) utility.
In this case download or copy the xiflash.exe file and floppy_bios.bin into your computer, boot to the DOS prompt and issue the following command:
```
xiflash -i floppy~1.bin -a c800 -p
```
and **reboot the computer**. Make sure to use the correct Multi-Floppy BIOS extension file name (floppy~1.bin in the example above), and the correct BIOS extension ROM address (segment - c800 in the example above). It should match the address set by JP5.2-JP4 jumpers
Once the computer reboots, follow the steps described above to configure and test the card.

Note: In order to save configuration and to re-program the Flash ROM in system, the ROM needs to be enabled (JP5.1 set) and write access allowed (JP5.7 set). It is possible to remove the latter once the card is configured to prevent accidental configuration change or Flash ROM re-programming/corruption.

__Congratulations! Enjoy your Monster FDC Controller Card!__
