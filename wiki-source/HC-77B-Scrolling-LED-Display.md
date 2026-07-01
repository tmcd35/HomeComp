# HC-77B Scrolling LED Display

## Release Identity

| Field | Detail |
|---|---|
| Product | HC Scrolling LED Display |
| Release wave | Mid 1978 |
| Display | 32×8 monochrome red dot matrix |
| Pixel count | 256 LEDs |
| Construction | Four close-fitted 8×8 LED modules on an exposed PCB/backplane |
| Power | Separate local supply and on/off switch |
| Host interface | Memory-mapped I/O extension |

The LED Display is a directly programmed peripheral. It has a 32-byte visible display store and an HC character-ROM lookup facility. It does not contain a text screen, autonomous scroll engine, or high-level graphics processor.

## Display Model

The 32 display bytes represent vertical columns from left to right:

```text
$F750 = leftmost display column
$F76F = rightmost display column
```

Within each column byte:

```text
bit 7 = top LED
bit 0 = bottom LED
```

A program can read or write every column immediately. The sign refreshes its LED matrix internally at a rate high enough to appear steady; scan timing is not part of the programming interface.

## I/O Map

| Address | Function | Access |
|---|---|---|
| `$F750-$F76F` | Display columns 0-31, left to right | Read/write |
| `$F770` | Font-select latch | Read/write |
| `$F771-$F778` | Font columns 0-7, left to right | Read only |

Writes to `$F771-$F778` are ignored.

## Power Behaviour

The extension is fitted through the normal emulator extension system but represents a separately powered physical board.

- On first attachment, local power is off.
- When off, reads from `$F750-$F778` return `$FF`; writes are ignored.
- A local off/on cycle clears all 32 display bytes and the font-select latch to `$00`.
- HC-77B warm reset does not clear the sign.
- Closing the sign window only hides the view; the device remains attached and active.

## Font ROM

Write an HC character code to `$F770`, then read its eight vertical glyph columns from `$F771-$F778`.

The sign uses the same character codes and artwork as the HC Keyboard/CRT display, stored in a column-oriented form suitable for the vertical LED columns. Unsupported character codes select the blank/NUL glyph.

A conventional right-to-left text scroll:

1. Copy display columns `$F751-$F76F` left into `$F750-$F76E`.
2. Read the next font byte from `$F771-$F778`.
3. Write that byte to `$F76F`.
4. Delay in software.

The HC-77B program controls all movement, timing, spacing, and animation.

## Demo Cassette

The supplied demo cassette is a base-1 KB machine-code example.

| Address | Meaning |
|---|---|
| `$0200` | Run current settings and message. |
| `$0380` | Restore `HOMECOMP - WELCOME` defaults, then run. |
| `$0050` | Scroll-delay outer-loop count. Larger values are slower. |
| `$0051` | Blank columns between message repetitions; default `$18` = 24. |
| `$0052` | Message length. `$00` clears the sign and returns to the monitor. |
| `$03C0-$03FF` | 64-byte user message buffer. |

The demo clears the sign at `$0200`, then scrolls exactly the number of message bytes named by `$0052`. It does not use a terminator or bounds check. Press `RST` to leave the running program through the normal HC-77B monitor reset path.

## Expansion-Bus Position

`$F750-$F778` is a released HC-77B device specification. Third-party extensions must not reuse this range.

See [HC-77B Expansion Bus API](HC-77B-Expansion-Bus-API) for extension packaging and address-claim rules.
