# Mid 1978 — Scrolling LED Display

The Mid 1978 release is the fourth public HomeComp ecosystem wave for the HC-77B.

It adds the **HC Scrolling LED Display**, a compact 32×8 red dot-matrix peripheral, and a demonstration cassette that runs on the 1 KB base HC-77B. It is the first official HC-77B display peripheral intended for directly programmed graphics as much as text.

## Release Contents

| Category | Released Material |
|---|---|
| Hardware | HC Scrolling LED Display extension |
| Software | Scrolling LED Demo Cassette |
| Documentation | Product and programming reference |
| Requirement | Base HC-77B with the LED Display fitted and locally powered |

## HC Scrolling LED Display

The display is one logical **32×8 pixel surface** made from four close-fitted 8×8 modules: 256 clear red LEDs in total. It is an exposed hobby-style board with its own local power switch and external power supply.

The device deliberately has no text buffer, automatic scrolling engine, speed control, or high-level graphics command set. The HC-77B program owns the animation. It can write any of the 32 visible display columns directly, read them back, shift them in either direction, and combine font data with its own graphics.

The extension uses the existing HC character artwork for its optional font lookup facility, so a character has the same visual identity on the Keyboard/CRT display and on the LED sign.

See [HC-77B Scrolling LED Display](HC-77B-Scrolling-LED-Display) for the released interface.

## Scrolling LED Demo Cassette

The supplied cassette is intentionally a small programming example rather than a menu-driven application.

It runs on the base 1 KB HC-77B. Users enter character bytes into a documented message buffer with the front-panel monitor, set the message length and timing values, then run the program. The program fetches each 8×8 glyph from the sign's font ROM and scrolls it right-to-left across the display.

| Entry | Purpose |
|---|---|
| `GO $0200` | Clear the sign and run the current user settings and message buffer. |
| `GO $0380` | Restore the supplied `HOMECOMP - WELCOME` demonstration settings and message, then run it. |

The Advanced Monitor Tools Cassette remains optional: it makes editing the message buffer more convenient on a 4 KB machine, but is not required by the LED demo.

## Release Role

The earlier TTY and Keyboard/CRT expansions make the HC-77B more usable as a programming system. The LED display does something different: it makes the machine feel visibly capable of driving an external project.

It provides:

- a practical 256-pixel output device for base-machine assembly programs
- a small, visual hardware project that still belongs to the 1978 hobby-computing world
- a direct demonstration of memory-mapped peripheral programming
- a distinctive non-terminal display for signs, simple animation, games, instruments, and experiments
- a third official reference point for the public expansion-bus API

## Related Releases

- [September 1977 Launch](September-1977-Launch)
- [Late 1977 — 3K RAM Expansion and NIM Cassette](Late-1977-RAM-Expansion-and-NIM)
- [March/April 1978 — Advanced Monitor Tools and HC Cassette BASIC](Easter-1978-Advanced-Monitor-and-Cassette-BASIC)
- [September 1978 — HC-PILOT and 8K ROM BASIC](September-1978-HC-PILOT-and-8K-ROM-BASIC)
