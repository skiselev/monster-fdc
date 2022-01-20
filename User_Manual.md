# Monster FDC - User Manual

## Introduction

Installation procedure outline:
* Prior to installing the Monster FDC controller card, configure the controller jumpers as described in the [Configuration Jumpers](#configuration-jumpers) section below
* Install card in your computer
* Power on the computer, launch built-in Multi-Floppy BIOS configuration utility, and configure controllers and drives there
* If needed, modify the OS configuration

## Configuration Jumpers

Monster FDC controller card is configured using jumpers. There are four blocks of jumpers that configure controller resources,
and two optional blocks that are used to connect +5V power supply to pin 3 of floppy disk connectors. Certain IBM PS/2 floppy drives are powered using this pin.

When configuring resources - I/O addresses, IRQs, DMA channels, and memory addresses for the BIOS extension ROM, make sure that resources are available and are not used
by other extension cards or built-in devices on the motherboard.

**Note:** Currently Multi-Floppy BIOS extension ROM does not support IRQ/DMA sharing between the primary FDC and the secondary FDC. Therefore do not use IRQ6 and DMA channel 2
for the secondary FDC

### JP1 - I/O Configuration

JP1 jumper block enables the primary and the secondary floppy disk controllers (FDCs), and enables and sets I/O address for the serial port (UART).

Jumper | Jumper Closed                                   | Jumper Open
-------|-------------------------------------------------|-------------------------------------
JP1.1  | Enable primary FDC (I/O address: 0x3F0)         | Disable primary FDC
JP1.2  | Enable secondary FDC (I/O address: 0x370)       | Disable secondary FDC
JP1.3  | Enable serial port at COM1 (I/O address: 0x3F8) | Do not configure serial port at COM1
JP1.4  | Enable serial port at COM2 (I/O address: 0x2F8) | Do not configure serial port at COM2
JP1.5  | Enable serial port at COM3 (I/O address: 0x3E8) | Do not configure serial port at COM3
JP1.6  | Enable serial port at COM4 (I/O address: 0x2E8) | Do not configure serial port at COM4

**Note:** Set only one of JP1.3 - JP1.6 jumpers, or no jumpers at all to disable serial port functionality

### JP2 - Secondary FDC IRQ Configuration

JP2 configures the hardware interrupt request (IRQ) for the secondary FDC. The primary FDC is hardwired to use IRQ6

Jumper | Jumper Closed                       | Jumper Open
-------|-------------------------------------|--------------------------------------
JP2.1  | Use IRQ6 for the secondary FDC      | Do not use IRQ6 for the secondary FDC
JP2.2  | Use IRQ4 for the secondary FDC      | Do not use IRQ4 for the secondary FDC
JP2.3  | Use IRQ3 for the secondary FDC      | Do not use IRQ3 for the secondary FDC
JP2.4  | Use IRQ5 for the secondary FDC      | Do not use IRQ5 for the secondary FDC
JP2.5  | Use IRQ7 for the secondary FDC      | Do not use IRQ7 for the secondary FDC
JP2.6  | Use IRQ2/IRQ9 for the secondary FDC | Do not use IRQ2/IRQ9 for the secondary FDC

**Notes:**
* Set only one of JP2.1 - JP2.6 jumpers
* Do not use IRQ6 setting, as it is used by the primary FDC, and currently the Multi-Floppy BIOS extension does not support IRQ sharing

### JP3 - Secondary FDC DMA Configuration

JP3 configures the DMA channel (DRQ and DACK) for the secondary FDC. The primary FDC is hardwired to use DMA channel 2

Jumpers         | Jumpers Closed                          | Jumpers Open
----------------|-----------------------------------------|-----------------------------------------------
JP3.1 and JP3.2 | Use DMA channel 2 for the secondary FDC | Do not use DMA channel 2 for the secondary FDC
JP3.3 and JP3.4 | Use DMA channel 1 for the secondary FDC | Do not use DMA channel 1 for the secondary FDC
JP3.5 and JP3.6 | Use DMA channel 3 for the secondary FDC | Do not use DMA channel 3 for the secondary FDC

**Notes:**
* Set only one pair of jumpers: either JP3.1 and JP3.2, JP3.3 and JP3.4, or JP3.5 and JP3.6
* Do not use DMA channel 2 setting, as it is used by the primary FDC, and currently the Multi-Floppy BIOS extension does not support DMA channel sharing

### JP4 - Serial Port IRQ Configuration

JP4 configures the hardware interrupt request (IRQ) for the serial port (UART). Note that JP3 shares a row with JP2 that is used to set IRQ for the secondary FDC.
This ensures that an IRQ is allocated to either the serial port or to the secondary FDC, but not to both of them at the same time.

Jumper | Jumper Closed                       | Jumper Open
-------|-------------------------------------|--------------------------------------
JP4.1  | Use IRQ4 for the serial port (COM1) | Do not use IRQ4 for the serial port
JP4.2  | Use IRQ3 for the serial port (COM2) | Do not use IRQ3 for the serial port
JP4.3  | Use IRQ5 for the serial port        | Do not use IRQ5 for the serial port
JP4.4  | Use IRQ7 for the serial port        | Do not use IRQ7 for the serial port
JP4.5  | Use IRQ2/IRQ9 for the serial port   | Do not use IRQ2/IRQ9 for the serial port

**Note:** Set only one of JP4.1 - JP4.5 jumpers

### JP5 - BIOS Extension ROM Configuration

JP5 enables BIOS extension ROM, enables or disables write access to the BIOS extension ROM, sets the ROM size and memory address

Jumper          | Jumper Closed                          | Jumper Open
----------------|----------------------------------------|--------------------------------------
JP5.1           | Enable BIOS extension ROM              | Disable BIOS extension ROM
JP5.2 - JP5.4   | Set BIOS extension ROM address         | See table below
JP5.5 and JP5.6 | Set 32 KiB ROM size (39SF010A, 28C256) | Set 8 KiB ROM size (28C64)
JP5.7           | Enable BIOS extension ROM writes       | Disable BIOS extension ROM writes

#### JP5.2 - JP5.4 - Configure BIOS Extension ROM Address

JP5.2  | JP5.3  | JP5.4  | ROM Address
-------|--------|--------|-----------------------------------------------------------------------
Open   | Open   | Open   | 0xC0000 (**Note:** This address will conflict with EGA/VGA Video BIOS)
Open   | Open   | Closed | 0xC8000
Open   | Closed | Open   | 0xD0000
Open   | Closed | Closed | 0xD8000
Closed | Open   | Open   | 0xE0000
Closed | Open   | Closed | 0xE8000

### JP6 - JP9 - Enable +5V on Pin 3 of Floppy Drive Connector

JP6 - JP9 are used to connect +5V power supply voltage to pin 3 of floppy drive connectors. Certain IBM PS/2 floppy drives are powered using this pin.

**Warning:** Do not connect jumpers unless you are certain that your floppy drive is powered using pin 3. Such drives normally do not have separate power connector.
If in doubt, check that pin 3 is not connected to the ground on the floppy drive. Setting this jumper with the floppy drive that grounds pin 3 will result in a short circuit
and possible damage to the floppy disk controller, floppy drive, motherboard, or power supply

Jumper | Jumper Closed                                   | Jumper Open
-------|-------------------------------------------------|--------------------------------------------------
JP6    | Connect +5V to pin 3 of J1 / Floppy 1 connector | Keep pin 3 of J1 / Floppy 1 connector unconnected
JP7    | Connect +5V to pin 3 of J2 / Floppy 2 connector | Keep pin 3 of J2 / Floppy 2 connector unconnected
JP8    | Connect +5V to pin 3 of J3 / Floppy 3 connector | Keep pin 3 of J3 / Floppy 3 connector unconnected
JP9    | Connect +5V to pin 3 of J4 / Floppy 4 connector | Keep pin 3 of J4 / Floppy 4 connector unconnected
