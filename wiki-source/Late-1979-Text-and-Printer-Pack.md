# Late 1979 — HC-77B Text & Printer Pack

> **Status: released.**

The Late 1979 release adds the **HC-77B Text & Printer Pack**, bringing text editing and hard-copy output to the fully expanded HC-77B.

The pack combines a 40-column impact printer peripheral with a resident Text Editor and printer-support ROM. It is aimed primarily at the 4K HC-77B Complete System configuration with Keyboard and Video Display Controller.

**Original price: £74.99.**

## Release Contents

| Category | Released Material |
|---|---|
| Hardware | HC-77B 40-column impact printer |
| Firmware | Text Editor and Printer ROM |
| Emulator | Printer extension and virtual printer window |
| Storage | Native HCT1 cassette document format |
| Documentation | Text Editor, printer and standalone programming instructions |
| Recommended system | HC-77B with 4K RAM, Keyboard and Video Display Controller |

## Text Editor

Enter the resident editor from the Monitor with:

```text
G C000
```

The editor provides a 14-row document display with separate message and command rows.

Commands include:

```text
A       Append line
D       Page down
U       Page up
E nn    Edit visible line
I nn    Insert before visible line
C nn    Clear/delete visible line
N       New document
S       Save document
L       Load document
P       Print complete document
```

Logical lines may contain up to 255 characters and scroll horizontally while editing. Documents can contain blank lines and long lines, and are saved to cassette using the native **HCT1** format.

Unsaved documents are protected before destructive New and Load operations.

## 40-Column Printer

The printer is modelled as a period 40-column impact printer with 5×7 dot-matrix output.

The emulator printer window provides:

- printer power control
- READY, ONLINE and ERROR state
- persistent virtual paper
- three-line manual paper feed
- PDF export
- host printing
- clear printed output

Printer power and mechanical state are emulated. Printing is deliberately slow: a normal printed line and paper advance takes approximately 2.83 seconds.

The editor automatically wraps long logical lines every 40 characters and preserves blank lines.

Press **ESC** during document printing to cancel at the next completed physical line.

## Standalone Printer Use

The printer is not restricted to the Text Editor.

Machine-code programs can use the stable printer ROM API for:

- character output
- CR
- LF
- CR/LF
- zero-terminated strings
- raw printer status

The fixed public entry points begin at `$C003`, while the printer hardware registers occupy `$CFFE-$CFFF`.

See the README supplied with the Text & Printer Pack for complete operating instructions and the standalone programming interface.

## Release Role

The Text & Printer Pack extends the HC-77B beyond programming and experimentation into simple practical document preparation.

Together with the 4K RAM expansion, keyboard, display, cassette storage and ROM BASIC, it represents one of the final major expansions of the original HC-77B platform before HomeComp moves toward its next generation of machines.
