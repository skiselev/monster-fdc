# Monster FDC - User Manual

## Introduction

Installation procedure outline:
* Prior to installing the Monster FDC controller card, configure the controller jumpers as described in the [Configuration Jumpers](#configuration-jumpers) section below
* Install card in your computer
* Power on the computer, launch built-in Multi-Floppy BIOS configuration utility to configure controllers, floppy drives, and Multi-Floppy BIOS options
* If needed, modify the OS configuration

## Configuration Jumpers

Monster FDC controller card is configured using jumpers. There are four blocks of jumpers that configure controller resources,
and two optional blocks that are used to connect +5V power supply to pin 3 of floppy disk connectors. Certain IBM PS/2 floppy drives are powered using this pin.

When configuring resources - I/O addresses, IRQs, DMA channels, and memory addresses for the BIOS extension ROM, make sure that resources are available and are not used
by other extension cards or built-in devices on the motherboard.

**Warning:** Currently Multi-Floppy BIOS extension ROM does not support IRQ/DMA sharing between the primary FDC and the secondary FDC. Therefore do not use IRQ6 and DMA channel 2
for the secondary FDC

### JP1 - I/O Configuration

JP1 jumper block enables the primary and the secondary floppy disk controllers (FDCs), and enables and sets I/O address for the serial port (UART).

Jumper | Closed                                          | Open
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

Jumper | Closed                              | Open
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

Jumpers         | Closed                                  | Open
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

Jumper | Closed                              | Open
-------|-------------------------------------|--------------------------------------
JP4.1  | Use IRQ4 for the serial port (COM1) | Do not use IRQ4 for the serial port
JP4.2  | Use IRQ3 for the serial port (COM2) | Do not use IRQ3 for the serial port
JP4.3  | Use IRQ5 for the serial port        | Do not use IRQ5 for the serial port
JP4.4  | Use IRQ7 for the serial port        | Do not use IRQ7 for the serial port
JP4.5  | Use IRQ2/IRQ9 for the serial port   | Do not use IRQ2/IRQ9 for the serial port

**Note:** Set only one of JP4.1 - JP4.5 jumpers

### JP5 - BIOS Extension ROM Configuration

JP5 enables BIOS extension ROM, enables or disables write access to the BIOS extension ROM, sets the ROM size and memory address

Jumper          | Closed                                 | Open
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

Jumper | Closed                                          | Open
-------|-------------------------------------------------|--------------------------------------------------
JP6    | Connect +5V to pin 3 of J1 / Floppy 1 connector | Keep pin 3 of J1 / Floppy 1 connector unconnected
JP7    | Connect +5V to pin 3 of J2 / Floppy 2 connector | Keep pin 3 of J2 / Floppy 2 connector unconnected
JP8    | Connect +5V to pin 3 of J3 / Floppy 3 connector | Keep pin 3 of J3 / Floppy 3 connector unconnected
JP9    | Connect +5V to pin 3 of J4 / Floppy 4 connector | Keep pin 3 of J4 / Floppy 4 connector unconnected

## Multi-Floppy BIOS Extension ROM

The Monster FDC card uses [Multi-Floppy BIOS Extension ROM](https://github.com/skiselev/floppy_bios) to provide support for the floppy drive formats that might otherwise not
be supported by the System BIOS, and to support more floppy drives that the System BIOS supports.

### Multi-Floppy BIOS Configuration Utility

The Multi-Floppy BIOS Extension ROM features a built-in configuration utility, that is used to configure floppy drive types connected to the controller card, and to set up the secondary FDC resources (IRQ, DMA channel). Press **F2** button during BIOS POST to access the configuration utility. The configuration utility has a simple command line interface with help prompt (press 'h' button to access help).

#### Enabling / Disabling Secondary FDC

Use '**e**' command to enable the secondary FDC. This command will prompt for the IRQ and the DMA channel numbers. If the secondary FDC is already enabled, it can be used to change IRQ and DMA settings.

Use '**d**' command to disable the secondary FDC. Note that disabling the secondary FDC will delete the configuration for all drives that are currently configured on the secondary FDC.

#### Adding / Deleting Drives

Use '**a**' command to add (configure) a floppy drive:
* If the secondary FDC is enabled, this command will prompt for the FDC the drive is connected to (primary or secondary)
* Next it will prompt for the physical drive number (0-3)
  * For the primary FDC:
    * Drive 0 is the drive at the far end of the cable (after the "twist") connected to J1 / Floppy 1 connector
    * Drive 1 is the drive in the middle of the cable (before the "twist") connected to J1 / Floppy 1 connector
    * Drive 2 is the drive at the far end of the cable (after the "twist") connected to J2 / Floppy 2 connector
    * Drive 3 is the drive in the middle of the cable (before the "twist") connected to J2 / Floppy 2 connector
  * For the secondary FDC:
    * Drive 0 is the drive at the far end of the cable (after the "twist") connected to J3 / Floppy 3 connector
    * Drive 1 is the drive in the middle of the cable (before the "twist") connected to J3 / Floppy 3 connector
    * Drive 2 is the drive at the far end of the cable (after the "twist") connected to J4 / Floppy 4 connector
    * Drive 3 is the drive in the middle of the cable (before the "twist") connected to J4 / Floppy 4 connector
* Then, it will prompt for the logical drive number.
  * The logical drive number will correspond to the drive number from software (BIOS calls) perspective
  * It roughtly corresponds to DOS drive letters - drive 0 = A:, drive 1 = B:, drives' letter assignments after drive B: depend on the DOS version. Newer DOS versions, use C: and following letters for hard drive partitions, and place any remaining floppy drives after hard drive letters. For example, on systems with four floppy drives and two HDD partitions, the drives will be assigned as following: A: - floppy drive 0, B: - floppy drive 1, C: - HDD partition 1, D: - HDD partition 2, E: - floppy drive 2, F: - floppy drive 3
  * The range of logical drive numbers depends on the number of drives already configured
    * Entering a number at the end of the range will add a drive
    * Entering a number in the middle will "insert" the drive, incrementing the logical drive numbers for the following drives
* Finally the configuration utility will prompt for the drive type

Use '**d**' command to delete a floppy drive. Note that deleting a drive will decrement the logical drive numbers for the following drives (if any).

#### Configuring Initial Program Loader (IPL) Method

Normally, at the end of Power-on Self Test (POST) the System BIOS calls INT 19h to boot the OS. The System BIOS normally has a default handler for INT 19h.
The functionality of the System BIOS handler depends on System / BIOS type. For example, IBM PC/XT and clones only support booting from floppy drives, and in certain cases will
not support boot from Multi-Floppy BIOS drives. IBM AT systems and clones will also attempt to boot from HDD. Some newer BIOSes have an option to setup the boot sequence.
In addition, other extension cards, for example SCSI / HDD controllers and Network Interface Cards might install their own INT 19h handler to support boot from these devices.

The Multi-Floppy BIOS support two IPL methods:
1. Built-in Multi-Floppy BIOS IPL. This method attempts to boot from the first (logical number 0) floppy drive. It should be used in IBM PC/XT and clone systems, or otherwise if boot from the floppy drive is desirable, but not working with System BIOS / HDD BIOS extensions.
2. System BIOS IPL. This method uses the default System BIOS (or an HDD BIOS extension) IPL. It should be used in IBM AT compatible systems

Use '**i**' command to configure IPL method.

#### Configuring Delays Type

The Multi-Floppy BIOS supports two delay methods:
1. AT delays. This method uses "refresh" bit in I/O Port 0x61. This bit is normally flipped every 15 microseconds, and allows for a precise timing of delays
2. XT delays. IBM PC/XT compatible systems do not have "refresh" bit, therefore when running on these systems, the BIOS uses a delay loop.

Use '**t**' command to configure delays type.

**Note:** It is important to configure the correct delay method. Using wrong delay type will cause the Multi-Floppy BIOS to fail. It will get "stuck" when using AT delays on XT systems, or floppy accesses might fail when using XT delays on AT systems (these systems are faster, and the delay loop will not produce long enough delays)

#### Other Commands

* Use '**p**' command to print the current configuration
* Use '**w**' command to write the configuration to the flash ROM / EEPROM and exit. Note that flash ROM / EEPROM needs to be writable - the JP5.7 jumper set, and on newer systems, the caching for the memory range where Multi-Floppy BIOS Extension ROM resides should be disabled
* Use '**q**' command to exit configuration utility without writing the configuration to the flash ROM / EEPROM
* Use '**h**' command to print help

**Warning:** Make sure to reboot the system after enabling the secondary FDC, configuring drives on the secondary FDC, and writing the configuration to flash ROM / EEPROM. This is required so that Multi-Floppy BIOS will properly configure the interrupt vectors required to support the secondary FDC

## DOS Configuration

It appears that by default, MS-DOS (tested on MS-DOS 6.22) does not support more than 4 floppy drives. Use DRIVER.SYS in CONFIG.SYS to add more drives.

The syntax is:
```
DEVICE=C:\DOS\DRIVER.SYS /D:number /F:type /C
```

Where:
* **number** specifies the logical drive number as assigned in the Multi-Floppy BIOS configuration utilty
* **type** specifies the floppy drive type: 
  * 0 - Double-density 360K, 5 1/4-inch floppy disk drives, including 160K, 180K, and 320K formatted media sizes
  * 1 - High-density, 1.2M, 5 1/4-inch floppy disk drives.
  * 2 - Double-density, 720K, 3 1/2-inch floppy disk drives.
  * 7 - High-density, 1.44M, 3 1/2-inch floppy disk drives.
  * 9 - Extended-density, 2.88M, 3 1/2-inch floppy disk drives.
* **/C** is an optional parameter indicating that the floppy drive supports "disk change" detection. Normally 1.2 MB, 1.44 MB, and 2.88 MB drives support this

When configuring multiple floppy drives, several "DRIVER.SYS" lines can be added into CONFIG.SYS

