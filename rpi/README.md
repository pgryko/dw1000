# Raspberry Pi DW1000 Integration

This directory contains the components necessary to integrate the DW1000 UWB radio with a Raspberry Pi. It includes:

- Circuit design files for a Raspberry Pi HAT (Hardware Attached on Top)
- Configuration files for system integration
- EEPROM programming utilities for HAT identification
- Raspberry Pi specific kernel module components

## Directory Structure

### circuit/

KiCad PCB and schematic design files for the Raspberry Pi HAT (Pi Tail) based on the DWM1000 module. This is a complete reference design that can be manufactured as a PCB.

### config/

Configuration files for system integration:
- `60-wpan.rules` - udev rules for automatically creating 6LoWPAN interfaces
- `dw1000.conf` - Module configuration for the DW1000 kernel module
- `interfaces-wpan` - Network interface configuration for wpan0 and lowpan0
- `Makefile` - Installation script for the configuration files

### eeprom/

Utilities and configuration for programming the HAT EEPROM:
- `piprog.sh` - Script for programming the HAT EEPROM
- `pitail.dts` - Device Tree Source file describing the HAT
- `idrom.dts` - Device Tree Source for EEPROM ID
- `pitail.txt` - Human-readable HAT description

### kernel/

Raspberry Pi specific kernel module files. This is essentially a wrapper around the main kernel module to facilitate building and installation on Raspberry Pi systems.

## Installation

1. Build the kernel module:
   ```bash
   git clone --depth 1 https://github.com/raspberrypi/linux rpi/kernel/linux
   KBUILD=/lib/modules/`uname -r`/build
   make -C ${KBUILD} M=`pwd`/rpi/kernel
   sudo make -C ${KBUILD} M=`pwd`/rpi/kernel modules_install
   sudo depmod -a
   ```

2. Install system configuration files:
   ```bash
   sudo make -C rpi/config install
   ```

3. Trigger udev to detect the DW1000:
   ```bash
   sudo udevadm trigger
   ```

## Hardware Setup

To use the DW1000 with a Raspberry Pi:

1. Either fabricate the HAT PCB using the KiCad files in the `circuit/` directory, or wire up a DWM1000 module according to the schematic.

2. For a custom HAT, program the EEPROM:
   ```bash
   cd rpi/eeprom
   make
   sudo ./piprog.sh pitail.eep
   ```

3. Connect the HAT to your Raspberry Pi's GPIO header.

4. Power on the Raspberry Pi; the module should be automatically detected.

5. Verify the interfaces are created:
   ```bash
   ip addr
   ```
   You should see `wpan0` and `lowpan0` interfaces listed.

## Network Configuration

The system creates two network interfaces:
- `wpan0`: Raw IEEE 802.15.4 monitor interface
- `lowpan0`: 6LoWPAN interface for IPv6 networking over IEEE 802.15.4

The `interfaces-wpan` configuration file ensures these interfaces are brought up automatically.
