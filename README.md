# monster-fdc
ISA floppy disk controller card that supports up to 8 floppy drives

## Introduction
Monster FDC is an ISA floppy disk controller board that features two FDCs. Each FDC supports up to four floppy drives. IRQ and DMA channel are configruable for the secondary FDC, and they are hardwired to IRQ6 and DMA2 for the primary FDC. The board also includes a serial port (UART) with configruable I/O address and IRQ.

<img src="images/Monster_FDC-Assembled_Board.jpg" alt="Monster FDC Assembled Board" height="600"> 

## User Manuals

[User Manual](User_Manual.md)

[Assembly Instructions](Assembly_Instructions.md)

## Hardware Documentation

### Schematic and PCB Layout

[Schematic - Version 1.0](KiCad/isa_monster_fdc-Schematic-1.0.pdf)

[PCB Layout - Version 1.0](KiCad/isa_monster_fdc-Board-1.0.pdf)

## Red Tape

### Licensing

Monster FDC is an open source hardware project certified by [Open Source Hardware Association](https://www.oshwa.org/), certification UID is [US00xxxx](https://certification.oshwa.org/us00xxxx.html). The hardware design itself, including schematic and PCB layout design files are licensed under the strongly-reciprocal variant of [CERN Open Hardware Licence version 2](license-cern_ohl_s_v2.txt). The [Multi-Flopppy BIOS](https://github.com/skiselev/floppy_bios) code is licensed under [GNU General Public License v3](license-gpl-3.0.txt). Documentation, including this file, is licensed under the [Creative Commons Attribution-ShareAlike 4.0 International License](license-cc-by-sa-4.0.txt).

![CERN-OHL-2.0-S, GPL-3.0, CC-BY-SA-4.0](images/CERN-OHL-2.0-S_GPL-3.0-only_CC-BY-SA-4.0.svg)
