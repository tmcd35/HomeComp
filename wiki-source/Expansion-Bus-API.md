# Expansion Bus API

This section documents the public HC-77B extension and peripheral API.

The API allows independently compiled extensions to add devices to the HC-77B emulator without being built into the main emulator binary. Extensions can register memory-mapped bus devices, declare the address ranges they occupy, expose interrupt lines, receive machine ticks and resets, and optionally register device windows for the emulator user interface.

## Current Scope

The current public API scope is the HC-77B.

| API Area | Status |
|---|---|
| Manifest-based extension loading | Supported |
| External extension assemblies | Supported |
| Bus device registration | Supported |
| Address claim validation | Supported |
| IRQ / NMI lines | Supported |
| Per-device tick and reset calls | Supported |
| Device window descriptors | Supported |
| Hot-plugging while powered on | Not supported |

## Current API Pages

- [HC-77B Expansion Bus API](HC-77B-Expansion-Bus-API)

## Design Intent

The HC-77B emulator treats extensions as attached hardware.

An extension should behave like a real peripheral connected before the machine is powered on. It should claim explicit address ranges, respond only to the addresses it owns, avoid hidden host-side shortcuts, and allow the emulated CPU to interact with it through normal memory-mapped reads and writes.

## Loading Rule

Extensions are loaded while the HC-77B is powered off.

This matches the machine model: peripherals are attached before use rather than hot-plugged into a running system.

## Public Release Rule

Only extension points that are part of the public HC-77B API should be used by third-party extension authors.

Internal emulator classes may exist for official devices, but third-party extensions should use the documented API surface described in this section.
