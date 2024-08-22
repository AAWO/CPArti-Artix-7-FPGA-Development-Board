# CPArti: Artix-7 FPGA Development Board

A fairly low-cost 4-layer AMD/Xilinx Artix-7 FPGA development board with bidirectional and unidirectional high-speed signals, SRAM, DAC and USB-C connection with built-in JTAG programmer. Designed for JLC04161H-7628 layer stackup in KiCad. All available components include JLCPCB parts numbers for assembly. The project includes [KiBot](https://github.com/INTI-CMNB/KiBot) configuration for documentation and production files generation automation.

The development board includes:
- Artix-7 FPGA
- USB-C connection to the FPGA via FTDI USB to JTAG (no need for additional programmer) + optional JTAG for a programmer (in case of USB connection failure)
- 2 unidirectional input and 3 unidirectional output SMA connections to FPGA
- 1 direct bidirectional SMA connections to FPGA
- 2 switched bidirectional SMA connections to a single FPGA pin (controlled from FPGA)
- additional SRAM (4Mb)
- high-speed DAC and op-amp for waveform generation implementation
- separate programmable delay line (delay value selected via DIP switches)
- 24 MHz FPGA clock
- 8-digit 7-segment (+ decimal point) displays with MAX7219 display driver
- 14 programmable LEDs
- 8 programmable push buttons (deglitched)
- 8 programmable slide switched
- powered via USB-C or separate 5.5mm DC barrel jack connector

## FTDI USB to JTAG configuration

The built-in FTDI FT232H chip can be used instead of AMD/Xilinx programmer in Vivado Hardware Manager. The FTDI device needs to be configured using Vivado's `program_ftdi` utility. Refer to the [Programming FTDI Devices for Vivado Hardware Manager Support](https://docs.amd.com/r/en-US/ug908-vivado-programming-debugging/Programming-FTDI-Devices-for-Vivado-Hardware-Manager-Support) for details.

## KiBot automation

### JLCPCB.kibot.yaml

The KiBot config to automatically generate production files:
- Gerber files
- drill files
- BOM (Bill of Materials)
- CPL (Component Placement List)

Usage:
```
kibot --schematic "CPArti FPGA dev board.kicad_sch" -c JLCPCB.kibot.yaml JLCPCB
```

### config.kibot.yaml

The KiBot config to automatically verify the design and generate documentation files.
- schematic
- PCB top side
- PCB bottom side
- PCB 3D view

The ERC (Electrical Rules Check) and DRC (Design Rules Check) are performed before each file generation.

Usage:
```
# schematic
kibot --schematic "CPArti FPGA dev board.kicad_sch" -c config.kibot.yaml print_sch

# PCB top & bottom side
kibot --schematic "CPArti FPGA dev board.kicad_sch" -c config.kibot.yaml print_PCB_top print_PCB_bottom

# PCB 3D view
kibot --schematic "CPArti FPGA dev board.kicad_sch" -c config.kibot.yaml 3D_top_view
```

## Main parts

### AMD/Xilinx Artix 7 FPGA

XC7A50T-FTG256
Datasheets:
[Artix 7 FPGAs Data Sheet: DC and AC Switching Characteristics (DS181)](https://docs.amd.com/v/u/en-US/ds181_Artix_7_Data_Sheet)
[7 Series FPGAs PCB Design Guide (UG483)](https://docs.amd.com/v/u/en-US/ug483_7Series_PCB)
[7 Series FPGAs Clocking Resources User Guide (UG472)](https://docs.amd.com/v/u/en-US/ug472_7Series_Clocking)
[7 Series FPGAs Configuration User Guide (UG470)](https://docs.amd.com/v/u/en-US/ug470_7Series_Config)
[xc7a35tcsg324 device pinout](https://www.xilinx.com/content/dam/xilinx/support/packagefiles/a7packages/xc7a35tcsg324pkg.txt)

### Hi-speed bidirectional and unidirectional SMA connections

Designed for clock signal phase alignment system
Reference: https://www.mdpi.com/2079-9292/13/16/3295

#### Terminations

Terminations are implemented as suggested in [7 Series FPGAs PCB Design Guide (UG483)](https://docs.amd.com/v/u/en-US/ug483_7Series_PCB). PCB cost (and therefore size) optimization is a more important objective than power efficiency. For this reason a Thevenin parallel termination is implemented, since generating a separate VTT=3.3V/2=1.65V would require more space and increase power places complexity.


### USB to JTAG

FTDI FT232H
Datasheet: https://ftdichip.com/wp-content/uploads/2020/07/DS_FT232H.pdf

### SRAM

IS61WV25616EDBLL
256k x 16 high speed asynchronous SRAM
Datasheet: https://www.issi.com/WW/pdf/61-64WV25616EDBLL.pdf

### Delay line

DS1023
Datasheet: https://www.analog.com/media/en/technical-documentation/data-sheets/DS1023.pdf

### DAC + op-amp

DAC: AD9744
Datasheet: https://www.analog.com/media/en/technical-documentation/data-sheets/AD9744.pdf

op-amp: OPA356
Datasheet: https://www.ti.com/lit/ds/symlink/opa356.pdf

### Display Driver

MAX7219
Datasheet: https://www.analog.com/media/en/technical-documentation/data-sheets/MAX7219-MAX7221.pdf

### Power

1. 5.0V DC-DC Converters: Texas Instruments TPS54561
Datasheet: https://www.ti.com/lit/ds/symlink/tps54561.pdf

2. 3.3V, 1.8V and 1.0V DC-DC Converters: Texas Instruments TPS62823
Datasheet: https://www.ti.com/lit/ds/symlink/tps62823.pdf

3. Power Sequencer: Texas Instruments LM3881
Datasheet: https://www.ti.com/lit/ds/symlink/lm3881.pdf

4. 2x auctioneering Schottky diode for dual supply (USB and barrel jack)


## Project name – CPArti

- **CPA** - [Clock signal Phase Alignment](https://www.mdpi.com/2079-9292/13/16/3295), from which the initial idea for the PCB emerged
- **Arti** - Artix-7 FPGA, the main part of the PCB
- **PArt** - each part of the dev board PCB is clearly marked


## Reference projects

- [asmi user's Artix-7 based DIY computer PCB project from EEVblog forum](https://www.eevblog.com/forum/fpga/planningdesignreview-for-a-6-layer-xilinx-artix-7-board-for-diy-computer/200/)
- [Antmicro Artix DC-SCM](https://opensource.antmicro.com/projects/artix-dc-scm/)
- [AMD VCK190 schematics](https://www.xilinx.com/products/boards-and-kits/vck190.html)
- [Digilent Arty A7 schematics](https://digilent.com/reference/_media/programmable-logic/arty-a7/arty-a7-e2-sch.pdf)
- [Digilent CMOD A7 schematics](https://digilent.com/reference/_media/reference/programmable-logic/cmod-a7/cmod_a7_sch_rev_c0.pdf)
