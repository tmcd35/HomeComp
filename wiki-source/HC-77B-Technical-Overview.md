# HC-77B Technical Overview

## Release Identity

The HC-77B is the first publicly released HomeComp machine.

| Field | Detail |
|---|---|
| Machine | HC-77B |
| Launch context | September 1977 launch system |
| Type | Front-panel single-board microcomputer |
| CPU | MOS Technology 6502 |
| Clock | 1 MHz |
| Primary interface | Front-panel display and keypad |
| Storage | Cassette interface |
| Release status | Publicly released |

The HC-77B launch release establishes the starting point for HomeComp Ecosystem Era 1.

![HC-77B system board overview](https://github.com/tmcd35/HomeComp/raw/master/site-assets/images/wiki/hc-77b-system-board.png)

*HC-77B system board overview.*

## Technical Summary

| Area | HC-77B Launch Specification |
|---|---|
| CPU | MOS Technology 6502 |
| Clock | 1 MHz |
| Base RAM | 1 KB |
| Planned RAM expansion | Up to 4 KB through the official RAM expansion path |
| ROM | Boot/monitor ROM included with the emulator release |
| Display | 8-digit seven-segment front-panel display |
| Input | Hexadecimal keypad with dedicated monitor keys |
| Cassette | Built-in cassette support in the emulator; blank tapes can be created |
| TTY expansion | Available at launch |
| Keyboard expansion | Available at launch |
| Video expansion | Available at launch |
| Expansion model | External expansion/peripheral support through the HC-77B expansion API |

## Machine Personality

The HC-77B is a front-panel-first machine.

It is operated through its hexadecimal keypad and 8-digit seven-segment display. The monitor ROM provides the basic control environment: inspecting memory, entering data, viewing registers, starting execution, and using cassette load/save functions.

The HC-77B should not be understood as a BASIC-first home computer. It begins as a monitor-controlled 6502 system, with richer software and hardware capability appearing through later ecosystem releases and expansion hardware.

For machine-level operation, the included HC-77B User Manual PDF is authoritative.

## Launch Configuration

The September 1977 launch release presents the HC-77B as a 1 KB base system, while making clear that official RAM expansion to 4 KB is part of the planned product path.

TTY, keyboard, and video expansions are available at launch. Cassette support is active in the emulator from launch, including the ability to create blank tapes.  

## High-Level Memory Map

The HC-77B uses a conventional 6502-style low-RAM, high-ROM arrangement.

| Address Range | Purpose | Notes |
|---|---|---|
| `$0000-$03FF` | Base RAM | 1 KB fitted in the launch machine |
| `$0400-$0FFF` | RAM expansion | Reserved for official RAM expansion up to 4 KB total |
| `$1000-$EFFF` | General expansion space | Reserved for expansion ROM, RAM, or peripheral use |
| `$F000-$F1FF` | Screen RAM | Used when video expansion is fitted |
| `$F200-$F6FF` | Reserved I/O and expansion space | Video, keyboard, peripheral, and future device space |
| `$F700-$F7FF` | Base I/O | Core machine I/O region |
| `$F800-$FFFF` | Monitor ROM | Boot and monitor ROM |

This wiki gives only the high-level memory map. The included user manual and technical documents should be treated as the authoritative released documentation for detailed operation.

## Front Panel

The HC-77B front panel provides the base machine interface.

| Component | Description |
|---|---|
| Display | 8-digit seven-segment LED display |
| Main input | Hexadecimal keypad |
| Function keys | Dedicated monitor-control keys |
| Operation style | Direct address/data/register interaction through the monitor ROM |

The front panel remains central even when optional launch expansions are present.

## Cassette Support

Cassette support is active in the emulator release.

The emulator provides cassette handling through the release's `tapes/` folder and supports the creation of blank tapes.

## Launch Expansions

The following expansion paths are available at launch:

| Expansion | Launch Position |
|---|---|
| TTY | Available |
| Keyboard | Available |
| Video | Available |
| RAM expansion | Planned official expansion path up to 4 KB total |
| Cassette media | Supported by the emulator |

The details of using emulator extensions and peripheral examples are covered in the [Expansion Bus API](Expansion-Bus-API) section.

## Emulator Support Status

The September 1977 emulator release supports the base HC-77B launch environment and includes the boot ROM required to start the machine.

| Feature | Status |
|---|---|
| HC-77B base machine | Supported |
| Boot ROM | Included |
| Front-panel operation | Supported |
| Cassette handling | Supported |
| Blank tape creation | Supported |
| TTY expansion | Available |
| Keyboard expansion | Available |
| Video expansion | Available |

## Related Released Documents

The emulator release includes the authoritative HC-77B machine documentation in PDF form.

Use the included documentation for detailed machine operation, worked examples, and front-panel monitor behaviour. This wiki page is intended only as a public technical overview.
