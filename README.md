
## Summary

This project is an SOC (System on a Chip) coded in VHDL and implemented for the Lattice iCE40-hx8k dev board. The SOC contains the following components: 6309 CPU + UART + Timer + I/O Ports

## Required Hardware

* Lattice iCE40-hx8k dev board (can be ordered online at www.latticesemi.com)
* USB-to-Serial 3.3V adapter (can be ordered from eBay)
* misc USB cables and wires for connecting the USB-to-Serial adapter

NOTE: Make sure the USB-to-serial adapter is a 3.3V version. Some adapters have 5V interface signals which could damage your iCE40-hx8k dev board.

## Tools

* IceCube2 (from Lattice Semiconductor) was used for synthesis and FPGA Routing.
* Icestorm (https:/github.com/cliffordwolf/icestorm) was used for programming.


## Build Flow

I used the Lattice IceCube2 software to generate the SOC_bitmap.bin programming file and then I used this command line "iceprog SOC_bitmap.bin" the program the iCE40-hx8k dev board over the USB cable (iceprog is part of the icestorm tool suite).

## Console Interface

I used the minicom program (on Ubuntu Linux) as a console to communicate with the 6309 SOC over the USB-to-Serial connection. Configure minicom using the command line "minicom -s" to configure the serial port for ttyUSB0 and turn of the hardware handshaking. There are probably other alternatives to minicom. Any ANSI terminal-emulator program should work for this application.

## Pinout

The iCE40 pins are defined as follows:
```
UART_RXD   G1 -pullup yes
UART_TXD   G2
PORTA[0]   B5
PORTA[1]   B4
PORTA[2]   A2
PORTA[3]   A1
PORTA[4]   C5
PORTA[5]   C4
PORTA[6]   B3
PORTA[7]   C3
RESET      N3 -pullup yes
CLK        J3 -pullup yes
```

## Memory Map

The memory map of this SOC is as follows:
```
$0000 -> $0007  UART registers
$0008 -> $000B  Timer registers
$000C           Output port register
$000D           Interrupt mask register
$000E -> 000F   Random number registers
$0010           Interrupt source register
$F000 -> $FFFF  Boot RAM (4k-bytes)
```

## 6309 CPU Background Info

The 6309 was introduced by Hitachi in the mid 1980s.
By default it operated in a 6809-compatible mode, but it could be switched to a more powerful native-6309 mode which featured:

+ Additional Registers:
  Two 8-bit accumulators: 'E' and 'F' which  can be concatenated
  to form 16-bit accumulator 'W'. In addition, the 16-bit accumulator D can be
  concatenated with W to form 32-bit accumulator 'Q'.

+ Additional Instructions:
  Most of the new instructions are modifications of existing
  instructions to handle the existence of the additional registers
  Other 6309 new instructions include inter-register arithmetic
  block transfers, hardware division, and bit-level manipulations.


## Contributors

* Scott L Baker - SOC design

## License

See the **LICENSE** file in this repository
