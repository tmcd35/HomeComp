# September 1978 — HC-77B 8K ROM BASIC

> **Status: released.**

The September 1978 release is the fifth public HomeComp ecosystem wave for the HC-77B.

It adds **HC-77B 8K ROM BASIC**, HomeComp’s first resident ROM programming environment and the final major language release for the original HC-77B platform.

## Release Contents

| Category | Released Material |
|---|---|
| Firmware | HC-77B 8K ROM BASIC |
| Emulator | Updated HC-77B emulator configuration |
| Documentation | ROM BASIC user and language documentation |
| Compatibility | HC Cassette BASIC program import |
| Native format | HCB4 cassette program format |
| Requirement | Base 1K HC-77B supported; 4K configuration recommended |

## Resident BASIC

HC ROM BASIC occupies an 8K ROM at `$D000-$EFFF`.

Unlike HC Cassette BASIC, the interpreter does not consume the main program area when loaded. Working RAM remains available for BASIC programs, variables and runtime storage.

The ROM automatically supports either:

- TTY Interface
- Keyboard and Video Display Controller

The same ROM image is used with both console configurations.

## Language

HC ROM BASIC extends the earlier cassette BASIC with:

- LLL-derived floating-point arithmetic
- numeric variables `A` to `M`
- fixed-length string variables
- mixed numeric and string `INPUT`
- mixed numeric and string `PRINT`
- `RND()`
- `LEN()`
- `ABS()`
- `INT()`
- hexadecimal numeric literals using `$`
- `GET(addr)`
- `PUT addr,value`
- `CALL addr`

The language deliberately retains a small expression grammar appropriate to an 8K 1978 ROM.

## Memory Access

The machine-level instructions make ROM BASIC useful for hardware experimentation and expansion programming.

```basic
PUT $F750,$FF
PRINT GET($F750)
CALL $0300

GET() reads a byte, PUT writes a byte, and CALL enters a 6502 machine-code routine. A normal 6502 RTS returns to BASIC.

## Program Compatibility

HC ROM BASIC introduces the native HCB4 cassette format.

Earlier HCB3 HC Cassette BASIC programs can be imported on a machine fitted with the 3K RAM Expansion. Imported programs are relocated into the ROM BASIC program area and may then be saved in HCB4 format.

ROM BASIC does not save programs in the older HCB3 format.

## System Requirements
| Configuration 	           | Support
|------------------------------|---------
| Base HC-77B with 1K RAM      | Supported with reduced variable and program capacity
| HC-77B with 3K RAM Expansion | Recommended full configuration
| TTY Interface	               | Supported
| Keyboard and Video Display Controller |	Supported
| Scrolling LED Display        | Directly accessible through GET() and PUT

Release Role

HC ROM BASIC completes the HC-77B’s progression from a front-panel development board into a compact general-purpose home computer.

The release provides:

a resident high-level language
floating-point arithmetic
cassette program storage
compatibility with the earlier BASIC generation
direct access to memory-mapped hardware
a practical bridge between BASIC and 6502 machine code
