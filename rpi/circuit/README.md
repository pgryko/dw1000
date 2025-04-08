# DW1000 Raspberry Pi HAT (Pi Tail) Circuit Design

This directory contains the KiCad PCB and schematic design files for a Raspberry Pi HAT (Hardware Attached on Top) featuring the DecaWave DWM1000 UWB radio module.

## Overview

The Pi Tail is a reference design that interfaces the DWM1000 module (which contains the DW1000 chip) with a Raspberry Pi through the GPIO header. The design follows the Raspberry Pi HAT specification, including the required EEPROM for identification and configuration.

## Design Files

- `pitail.pro` - KiCad project file
- `pitail.sch` - Schematic design
- `pitail.kicad_pcb` - PCB layout
- `pitail-cache.lib` - Library cache
- `fp-lib-table` - Footprint library table

Additional libraries are organized in subdirectories:
- `lib/cvra/` - Component library
- `lib/pihat/` - Raspberry Pi HAT specific components
- `lib/pitail/` - Custom components for the Pi Tail design

## Features

- DWM1000 UWB module integration
- Raspberry Pi HAT form factor
- SPI interface to the Raspberry Pi
- GPIO connections for IRQs and control signals
- EEPROM for HAT identification
- Power regulation
- Status LEDs

## Technical Specifications

- **SPI Interface**: Connected to the Raspberry Pi SPI0 bus
- **IRQ**: DW1000 IRQ connected to a GPIO pin for interrupt handling
- **Reset & Power Control**: Connected to GPIOs for software control
- **Power Supply**: 3.3V supplied from the Raspberry Pi GPIO header
- **EEPROM**: I2C EEPROM at address 0x50 for HAT identification

## Schematic Overview

The schematic includes:
1. DWM1000 module connections
2. HAT EEPROM circuit
3. Power regulation
4. GPIO connection mapping
5. Status LEDs

## PCB Layout

The PCB is designed to the Raspberry Pi HAT mechanical specification:
- Dimensions: 65mm x 56mm
- Mounting holes at standard HAT positions
- 40-pin GPIO header connector

## Manufacturing

The design can be manufactured using standard PCB fabrication processes. The BOM cost is approximately $30-$50 in single quantities, including:
- PCB manufacturing
- DWM1000 module
- EEPROM
- Passive components
- Connectors

## Usage

To open and modify the design:

1. Open the project in KiCad (version 5.0 or later)
2. The schematic can be opened with KiCad's Eeschema
3. The PCB layout can be opened with KiCad's Pcbnew

To manufacture the board:
1. Generate Gerber files from KiCad's Pcbnew
2. Generate drill files
3. Send these files to your preferred PCB manufacturer
4. Assemble the components according to the BOM and schematic

## Assembly Notes

- The DWM1000 module is a surface-mount component with castellated edges
- Use a reflow oven or hot air station for the DWM1000 module
- Hand soldering is suitable for the remaining components
- Program the EEPROM using the tools in the `rpi/eeprom/` directory after assembly
