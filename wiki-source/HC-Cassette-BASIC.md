# HC Cassette BASIC

HC Cassette BASIC is HomeComp’s first BASIC language for the expanded HC-77B.

It is a compact cassette-loaded integer BASIC designed for the machine’s 4 KB working configuration. It is supplied in TTY and Keyboard/CRT variants, using the same stored-program cassette format.

## Requirements

| Variant            | Required Hardware                                               |
| ------------------ | --------------------------------------------------------------- |
| TTY build          | HC-77B, 3K RAM Expansion, TTY Interface                         |
| Keyboard/CRT build | HC-77B, 3K RAM Expansion, Keyboard and Video Display Controller |

## Loading and Starting

Load HC Cassette BASIC through the HC-77B ROM monitor at `$0200`, then use:

```text
GO $0200
```

A cold start displays:

```text
HC-CASSETTE BASIC
]
```

During execution, front-panel `RST` returns to the ROM monitor. Use `GO $0200` for a warm return to BASIC; the stored program and variables are retained.

## BASIC Shell Commands

| Command            | Meaning                        |
| ------------------ | ------------------------------ |
| `10 PRINT "HELLO"` | Add or replace a numbered line |
| `10`               | Delete a line                  |
| `G`                | Run program                    |
| `N`                | Clear program                  |
| `D`                | List program                   |
| `S`                | Save program to cassette       |
| `L`                | Load program from cassette     |

Listing pauses after fifteen lines with `?CONT`. Press `Y` to continue.

## Supported BASIC

```text
STOP
GOTO line
LET A=expression
IF value relation value THEN line
INPUT A
PRINT
PRINT "TEXT"
PRINT expression
```

Variables are `A` to `M`. Values are signed 16-bit integers.

Arithmetic supports one operation per expression:

```text
+
-
*
/
```

Relations are:

```text
=
#
<
>
```

`#` means “not equal”.

## Limits

* 31 characters per entered source line
* 384 bytes of tokenised program storage
* integer arithmetic only
* overflow wraps
* `PRINT` always ends with a new line
* `IF` compares simple values only

There are no arrays, string variables, `DIM`, `FOR/NEXT`, `GOSUB/RETURN`, `REM`, `END`, brackets, operator precedence, chained arithmetic, or multi-item `PRINT` and `INPUT`.

## Related Pages

* [Easter 1978 — Advanced Monitor Tools and HC Cassette BASIC](Easter-1978-Advanced-Monitor-and-Cassette-BASIC)
* [Advanced Monitor Tools Cassette](Advanced-Monitor-Tools-Cassette)
* [HC-77B Technical Overview](HC-77B-Technical-Overview)
