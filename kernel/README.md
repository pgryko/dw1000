# DW1000 Linux Kernel Module

This directory contains the Linux kernel module for the DecaWave DW1000 IEEE 802.15.4 UWB (Ultra-Wideband) radio chip.

## Overview

The DW1000 is a wireless transceiver module from DecaWave (now Qorvo) that implements the IEEE 802.15.4-2011 UWB standard. This kernel module provides a driver that integrates the DW1000 with the Linux IEEE 802.15.4 subsystem and implements high-precision timestamping via a PTP (Precision Time Protocol) clock.

## Files

- `dw1000.c` - The main driver implementation
- `dw1000.h` - Header file with register definitions, data structures, and function prototypes
- `timehires.c` - High-resolution time counter implementation for precise timestamping
- `timehires.h` - Header for the high-resolution time counter
- `kcompat.h` - Kernel compatibility header for handling different kernel versions
- `inject.h` - Error injection support for testing (only used when `DW1000_ERROR_INJECT` is defined)
- `Makefile` - Build script for the kernel module

## Features

- IEEE 802.15.4 frame transmission and reception
- Precise packet timestamping with PTP clock support
- Hardware monitoring (voltage and temperature)
- Configurable radio parameters (channel, PRF, data rate, etc.)
- Support for calibration profiles

## Building

The module can be built by running `make` with appropriate kernel headers:

```bash
KBUILD=/lib/modules/$(uname -r)/build make -C $KBUILD M=$(pwd)
```

## Hardware Requirements

- SPI bus connection to the DW1000 chip
- GPIO pins for reset and power control (optional)
- Interrupt pin for IRQ handling

## Module Parameters

- `tsinfo` - Timestamp information enable flags (default: 0)
  - Bit 0: Enable raw timestamp info
  - Bit 1: Enable RX quality info
  - Bit 2: Enable time tracking info
  - Bit 3: Enable ADC readings (temperature, voltage)

## Sysfs Interface

The driver exposes various configuration and status parameters through sysfs:

- `/sys/devices/.../dw1000/profile` - Calibration profile management
- `/sys/devices/.../dw1000/xtalt` - Crystal trim value
- `/sys/devices/.../dw1000/channel` - Radio channel configuration
- `/sys/devices/.../dw1000/pcode` - Preamble code selection
- `/sys/devices/.../dw1000/prf` - Pulse repetition frequency
- `/sys/devices/.../dw1000/antd` - Antenna delay calibration
- `/sys/devices/.../dw1000/rate` - Data rate selection
- `/sys/devices/.../dw1000/txpsr` - Transmit preamble symbol repetitions
- `/sys/devices/.../dw1000/tx_power` - Transmit power levels
- `/sys/devices/.../dw1000/smart_power` - Smart power control enable/disable
- `/sys/devices/.../dw1000/frame_filter` - Frame filtering configuration
- `/sys/devices/.../dw1000/snr_threshold` - Signal-to-noise ratio threshold
- `/sys/devices/.../dw1000/fpr_threshold` - First path ratio threshold
- `/sys/devices/.../dw1000/noise_threshold` - Background noise threshold
- `/sys/devices/.../dw1000/stats_interval` - Statistics reporting interval

## Hardware Monitor

The driver registers with the Linux hwmon subsystem to provide:
- Voltage monitoring
- Temperature monitoring
