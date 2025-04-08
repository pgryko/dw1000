# Raspberry Pi DW1000 Configuration Files

This directory contains the configuration files needed to properly integrate the DW1000 UWB radio with the Raspberry Pi's operating system.

## Files

### 60-wpan.rules

This is a udev rule that automatically creates a 6LoWPAN interface for any IEEE 802.15.4 PHY device when it is detected by the system. The rule:

1. Watches for network subsystem events
2. Filters for devices that are part of the IEEE 802.15.4 subsystem
3. Triggers only on "add" actions
4. Runs a command to create a 6LoWPAN interface linked to the IEEE 802.15.4 interface

The created interface will be named `lowpan0` (for an interface named `wpan0`).

### dw1000.conf

This is a module configuration file that sets options for the DW1000 kernel module. The options:

- `tsinfo=0x0f` - Enables all timestamp information flags (all 4 bits set):
  - Bit 0: Enable raw timestamp info
  - Bit 1: Enable RX quality info
  - Bit 2: Enable time tracking info
  - Bit 3: Enable ADC readings (temperature, voltage)

When installed, this file goes to `/etc/modprobe.d/` where it's read by the module loading system.

### interfaces-wpan

This is a network interface configuration fragment for the Debian/Ubuntu `ifupdown` system. It defines:

1. Both `wpan0` and `lowpan0` interfaces as hot-pluggable
2. Configuration for the `wpan0` interface (IEEE 802.15.4 monitor interface)
3. Configuration for the `lowpan0` interface (6LoWPAN interface for IPv6)

When installed, this file goes to `/etc/network/interfaces.d/wpan` where it's included in the network configuration.

### Makefile

A simple Makefile to install the configuration files to their proper locations:

- `interfaces-wpan` → `/etc/network/interfaces.d/wpan`
- `60-wpan.rules` → `/etc/udev/rules.d/60-wpan.rules`
- `dw1000.conf` → `/etc/modprobe.d/dw1000.conf`

After installation, it also reloads the udev rules with `udevadm control --reload`.

## Usage

To install all the configuration files:

```bash
sudo make -C rpi/config install
```

This will install the files and reload udev rules. After installation, you'll need to:

1. Ensure the kernel module is loaded
2. Trigger udev to detect the DW1000 device: `sudo udevadm trigger`
3. Verify interfaces are created: `ip addr` (should show wpan0 and lowpan0)
