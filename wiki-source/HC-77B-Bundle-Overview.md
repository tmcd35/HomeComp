# HC-77B Technical Overview

## Release Identity

The HC-77B is the first publicly released HomeComp machine.

| Field | Detail |
|---|---|
| Machine | HC-77B |
| Launch context | Easter 1979 Complete System Bundle |
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
| RAM | 1 KB  Base + 3 KB expansion |
| ROM | Boot/monitor ROM included with the emulator release |
| BASIC | 8 KB ROM expansion included in bundle |
| Display | 8-digit seven-segment front-panel display |
| Input | Hexadecimal keypad with dedicated monitor keys |
| Cassette | Built-in cassette support in the emulator; blank tapes can be created |
| Keyboard expansion | Included in bundle |
| Video expansion | Included in bundle |
| Expansion model | External expansion/peripheral support through the HC-77B expansion API |

## Machine Personality

The HC-77B is a front-panel-first machine.

It is operated through its hexadecimal keypad and 8-digit seven-segment display. The monitor ROM provides the basic control environment: inspecting memory, entering data, viewing registers, starting execution, and using cassette load/save functions.

The HC-77B should not be understood as a BASIC-first home computer. It begins as a monitor-controlled 6502 system, with richer software and hardware capability appearing through later ecosystem releases and expansion hardware.

For machine-level operation, the included HC-77B User Manual PDF is authoritative.

## Bundle Configuration

This release provides a complete system with 4 KB total RAM, an 8K BASIC ROM, Display adaptor with RF connection to a dometic TV and an ASCII keyboard attachment.

The bundle is designed to give a complete usable home computer setup.  It boots directly to the standard HC77 system ROM.  For BASIC enter address $D000 and press GO.

## High-Level Memory Map

The HC-77B uses a conventional 6502-style low-RAM, high-ROM arrangement.

| Address Range | Purpose | Notes |
|---|---|---|
| `$0000-$03FF` | Base RAM | 1 KB fitted in the launch machine |
| `$0400-$0FFF` | 3K RAM Expansion |
| `$1000-$EFFF` | General expansion space | Reserved for expansion ROM, RAM, or peripheral use |
| `$D000-$EFFF` | 8K BASIC ROM
| `$F000-$F1FF` | Screen RAM for CRT Dispay |
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

## Available Expansions

The following system expansions available seperately:

- TTY Interface
- Scrolling LED Display

The details of using emulator extensions and peripheral examples are covered in the [Expansion Bus API](Expansion-Bus-API) section.


## Related Released Documents

The emulator release includes the authoritative HC-77B machine documentation in PDF form.

Useful repository files:

- [HC-77B Product Sheet PDF](https://github.com/tmcd35/HomeComp/blob/master/documents/User/HC77B/hc_77b_product_sheet.pdf)
- [HC-77B Product Sheet Image](https://github.com/tmcd35/HomeComp/blob/master/documents/advertising/HC-77B%20Product%20Sheet.png)
- [HC-77B Technical Specifications](https://github.com/tmcd35/HomeComp/blob/master/documents/User/HC77B/Technical%20Specifications.docx)

Use the included documentation for detailed machine operation, worked examples, and front-panel monitor behaviour. This wiki page is intended only as a public technical overview.
