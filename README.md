# DW1000 UWB Radio Driver and Reference Design

## Overview

This repository contains a Linux kernel module for the DecaWave DW1000 ultra-wideband (UWB) radio chip, along with a reference design for a Raspberry Pi HAT (Hardware Attached on Top) PCB based on the DWM1000 module. The DW1000 is an IEEE 802.15.4 UWB radio chip capable of timestamping received and transmitted packets with a nominal resolution of 64GHz (15ps), allowing for distance measurements with theoretical precision of around 0.5cm.

## Repository Structure

- **`kernel/`** - The core Linux kernel module for the DW1000 radio
  - Driver implementation, headers, and high-resolution timing support

- **`rpi/`** - Raspberry Pi specific components
  - **`circuit/`** - KiCad PCB design files for the Raspberry Pi HAT
  - **`config/`** - Configuration files for udev rules and network interfaces
  - **`eeprom/`** - HAT EEPROM configuration files
  - **`kernel/`** - Raspberry Pi kernel module integration

## Key Features

- **IEEE 802.15.4 Compliant**: Implements the standard for low-rate wireless personal area networks (LR-WPANs)
- **Ultra-Wideband**: Uses UWB technology for high precision ranging and positioning
- **High-Resolution Timestamping**: 64GHz nominal resolution (15ps) for precise time-of-flight measurements
- **PTP Clock Support**: Integrates with Linux's PTP (Precision Time Protocol) subsystem
- **Complete Hardware Design**: Includes a full reference design for a Raspberry Pi HAT

## Hardware Reference Design

The repository includes a reference design for a Raspberry Pi HAT PCB based on the DWM1000 module. The total material cost in single-unit quantities is around $30-$50 per board (including PCB manufacturing).

## Installation Instructions

### Prerequisites

- Raspberry Pi with recent Raspberry Pi OS
- Kernel headers installed

### Building and Installing

1. Update and install kernel headers:
   ```bash
   sudo apt update
   sudo apt install raspberrypi-kernel raspberrypi-kernel-headers
   ```

2. Build and install kernel modules:
   ```bash
   git clone --depth 1 https://github.com/raspberrypi/linux rpi/kernel/linux
   KBUILD=/lib/modules/`uname -r`/build
   make -C ${KBUILD} M=`pwd`/rpi/kernel
   sudo make -C ${KBUILD} M=`pwd`/rpi/kernel modules_install
   sudo depmod -a
   ```

3. Install configuration files:
   ```bash
   sudo make -C rpi/config install
   ```

4. Trigger udev to detect the DW1000:
   ```bash
   sudo udevadm trigger
   ```

5. Verify network interfaces:
   ```bash
   ip addr
   ```
   You should see two interfaces: `wpan0` and `lowpan0`

## Applications

The DW1000 UWB radio can be used for:
- Indoor positioning systems
- Distance/range measurement
- Asset tracking
- Wireless sensor networks
- Time-sensitive networking

## Technical Specifications

- **Ranging Precision**: Sub-centimeter theoretical resolution (practical resolution may vary)
- **Operating Channels**: Channels 1, 2, 3, 4, 5, and 7 in the UWB spectrum
- **Data Rates**: 110kbps, 850kbps, and 6.8Mbps
- **Interface**: SPI bus connection to host system

## License

This project is licensed under the GNU General Public License v2 or later.

## Credits

This driver was developed by:
- Michael Brown <mbrown@fensystems.co.uk>
- Petri Mattila <petri.mattila@unipart.io>
