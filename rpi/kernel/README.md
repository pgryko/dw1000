# Raspberry Pi DW1000 Kernel Module

This directory contains the Raspberry Pi-specific kernel module build files for the DW1000 UWB radio driver.

## Overview

While the core DW1000 driver implementation resides in the main `kernel/` directory at the root of this repository, this directory provides the build system integration specifically for the Raspberry Pi platform. It essentially acts as a wrapper around the main kernel module to facilitate building and loading on Raspberry Pi systems.

## Directory Structure

- `Makefile` - Build script for compiling the kernel module for Raspberry Pi
- `dw1000/` - Symbolic links to the core DW1000 driver implementation files

## Building and Installing

To build and install the kernel module:

1. Ensure you have the Raspberry Pi kernel headers installed:
   ```bash
   sudo apt update
   sudo apt install raspberrypi-kernel raspberrypi-kernel-headers
   ```

2. Clone the Raspberry Pi Linux kernel repository (only needed for build references):
   ```bash
   git clone --depth 1 https://github.com/raspberrypi/linux rpi/kernel/linux
   ```

3. Build the kernel module:
   ```bash
   KBUILD=/lib/modules/`uname -r`/build
   make -C ${KBUILD} M=`pwd`/rpi/kernel
   ```

4. Install the kernel module:
   ```bash
   sudo make -C ${KBUILD} M=`pwd`/rpi/kernel modules_install
   ```

5. Update the module dependencies:
   ```bash
   sudo depmod -a
   ```

## Loading the Module

After installation, the module can be loaded with:

```bash
sudo modprobe dw1000_mod
```

To check if the module is loaded:

```bash
lsmod | grep dw1000
```

## Customizing Module Parameters

You can customize module parameters either:

1. At load time:
   ```bash
   sudo modprobe dw1000_mod tsinfo=0x0f
   ```

2. Via configuration file (preferred method):
   Edit `/etc/modprobe.d/dw1000.conf` (or install the one from `rpi/config/`):
   ```
   options dw1000_mod tsinfo=0x0f
   ```

## Debugging

For debugging purposes, the module is built with the `DEBUG` flag enabled by default. This provides detailed logging in the kernel log buffer, which can be viewed with:

```bash
sudo dmesg | grep dw1000
