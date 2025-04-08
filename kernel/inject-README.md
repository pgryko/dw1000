# DW1000 Error Injection Framework

The `inject.h` file provides a framework for injecting controlled errors into the DW1000 driver for testing error handling and recovery mechanisms.

## Overview

Testing error handling in device drivers is challenging because hardware errors are often difficult to reproduce. The error injection framework allows developers to simulate various error conditions programmatically, helping to verify that the driver's error handling code works correctly.

## Features

This framework allows for the injection of various types of errors:

- State machine timing delays
- Corruption of system status register values
- Simulated SPI bus read/write failures
- Transmission and reception delays
- Configuration errors

## Usage

The error injection mechanism is only enabled when the driver is compiled with the `DW1000_ERROR_INJECT` macro defined. In normal production builds, this feature is disabled to avoid any performance impact.

To use error injection:

1. Ensure the driver is compiled with `DW1000_ERROR_INJECT` defined
2. Set the error type and value through the sysfs interface:
   ```
   echo "ERROR_TYPE:VALUE" > /sys/devices/.../dw1000/error_inject
   ```
   For example:
   ```
   echo "STATE_DELAY:100" > /sys/devices/.../dw1000/error_inject
   ```

## Available Error Types

The following error types are defined:

| Error Type | Description |
|------------|-------------|
| `NONE` | No error (default state) |
| `STATE_DELAY` | Introduces a delay in the state machine processing |
| `STATE_SYS_STATUS` | Modifies the system status register value |
| `TX_DELAY` | Adds a delay during transmission |
| `TX_SPI_DATA` | Simulates an SPI error during data transmission |
| `TX_SPI_INFO` | Simulates an SPI error during transmit information retrieval |
| `RX_DELAY` | Adds a delay during reception |
| `RX_SYS_STATUS` | Modifies the receive system status register value |
| `SPI_RD_ERROR` | Simulates an SPI read error |
| `SPI_WR_ERROR` | Simulates an SPI write error |
| `CONFIG_ERROR` | Simulates an error during configuration |

## Implementation Notes

- The error injection is one-shot - after an error is injected, the mechanism resets to the `NONE` state
- Errors can be injected at various points in the driver code through strategically placed `INJECT_ERROR()` macro calls
- For errors that require a value (e.g., specific delay time, error code), the value can be specified after a colon in the error type string

## Example

To inject a 500ms delay in the state machine:

```
echo "STATE_DELAY:500" > /sys/devices/.../dw1000/error_inject
```

To simulate an SPI read error with error code -5 (EIO):

```
echo "SPI_RD_ERROR:-5" > /sys/devices/.../dw1000/error_inject
```

## Caution

This feature is intended for development and testing only. Using error injection in production environments can cause unexpected behavior, including:

- Packet loss
- System instability
- Device reset
- Network interface failures

Always disable error injection in production builds by not defining `DW1000_ERROR_INJECT`.
