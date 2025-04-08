# Raspberry Pi HAT EEPROM Configuration

This directory contains files and utilities for configuring and programming the EEPROM on a Raspberry Pi HAT (Hardware Attached on Top) for the DW1000 UWB radio.

## Overview

Raspberry Pi HATs include an EEPROM that stores information about the board, including its identity, GPIO setup, and device tree overlay. This allows the Raspberry Pi to automatically configure itself when the HAT is connected.

## Files

### Makefile

A build script that compiles the device tree source (DTS) files into DTB (Device Tree Blob) format and then converts them to the binary format needed for the EEPROM.

### piprog.sh

A shell script for programming the HAT EEPROM. Usage:

```bash
sudo ./piprog.sh pitail.eep
```

The script handles:
- Loading required kernel modules (i2c_dev, at24)
- Writing the binary image to the EEPROM at the specified I2C address
- Verifying the written data
- Cleaning up by unloading modules

Environment variables:
- `CHIP`: EEPROM chip type (default: 24c64)
- `I2C_BUS`: I2C bus number (default: 0)
- `I2C_ADDR`: I2C device address (default: 50)

### pitail.dts

The main device tree source file that describes the HAT hardware configuration. It includes:
- HAT identification information
- GPIO pin usage and configuration
- Device-specific configuration for the DW1000

### idrom.dts

A simplified device tree source file that contains just the basic identification information for the HAT.

### pitail.txt

A human-readable description of the HAT that includes:
- Product name and description
- Manufacturer information
- Product revision details
- Operating parameters

This text is embedded in the EEPROM to allow tools to display user-friendly information about the HAT.

## Usage

To build and program the EEPROM:

1. Build the EEPROM image:
   ```bash
   make
   ```
   This will create `pitail.eep` (or other .eep files depending on your targets)

2. Program the EEPROM:
   ```bash
   sudo ./piprog.sh pitail.eep
   ```

3. Verify the programming was successful:
   - The script will return 0 if verification passed
   - You can also read back the EEPROM contents to check:
     ```bash
     sudo modprobe i2c_dev
     sudo modprobe at24
     sudo sh -c 'echo 24c64 0x50 > /sys/class/i2c-adapter/i2c-0/new_device'
     sudo hexdump -C /sys/class/i2c-adapter/i2c-0/0-0050/eeprom | head
     ```

## Hardware Notes

The EEPROM should be connected to the Raspberry Pi's I2C bus with:
- Device address: 0x50
- Write protect pin (WP) connected to ground or a GPIO for write control
- Compatible with the 24c64 EEPROM specification
