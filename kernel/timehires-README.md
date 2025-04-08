# High-Resolution Time Counter

The files `timehires.c` and `timehires.h` implement a high-resolution time counter for the DW1000 driver. This component is essential for the precise timestamp functionality needed in UWB ranging applications.

## Overview

The High-Resolution Time Counter provides a way to maintain precise nanosecond-level timing, which is critical for accurate distance measurements using time-of-flight in UWB systems. It extends the Linux timing system to handle fractional nanoseconds and manage high-frequency hardware counters.

## Key Components

- `struct timehires` - A high-resolution timestamp structure with nanosecond and fractional parts
- `struct hires_counter` - Hardware abstraction for a free-running counter, including:
  - Time synchronization support
  - Cycle counting and mask handling
  - Multiplier management for time conversion

## Features

- High-precision timestamp conversion between hardware cycles and nanoseconds
- Support for hardware cycle counter wrap-around
- Multiplier adjustment for frequency tuning
- Monotonic time maintenance
- Proper handling of before/after timestamps relative to counter synchronization

## Usage

The DW1000 driver uses this component to convert the hardware's 40-bit cycle counter (running at approximately 64GHz) to Linux kernel timestamps. This enables:

1. Precise packet timestamping for IEEE 802.15.4 frames
2. PTP (Precision Time Protocol) clock implementation
3. Time-of-flight calculations for distance measurement

## Implementation Notes

- The implementation is based on Linux kernel's timecounter.c but extended for higher precision
- Mathematical operations carefully handle 64-bit multiplications and overflow conditions
- The counter handles the case where the hardware counter "wraps around" its maximum value
- Time adjustments maintain monotonicity to prevent time going backward
