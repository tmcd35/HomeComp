# Advanced Monitor Tools Cassette

The Advanced Monitor Tools Cassette is HomeComp’s machine-code programming package for the expanded HC-77B.

It requires the official 3K RAM Expansion and is supplied in two environment-specific variants:

| Cassette Side | Environment                           |
| ------------- | ------------------------------------- |
| Side A        | TTY Interface                         |
| Side B        | Keyboard and Video Display Controller |

## Contents

| Component           | Purpose                                           |
| ------------------- | ------------------------------------------------- |
| HC Advanced Monitor | Command-driven monitor and machine-code workbench |
| HC EDIT             | Compact source-text editor                        |
| HC ASM              | Two-pass 6502 assembler                           |

## Product Role

The package gives the HC-77B a practical software-development path without changing its front-panel identity. Users still load the tools from cassette through the ROM monitor, but once loaded they gain a richer text-based environment for inspecting memory, editing source, assembling programs, and returning to the machine monitor when required.

## Requirements

* HC-77B
* Official 3K RAM Expansion
* TTY Interface for Side A, or Keyboard and Video Display Controller for Side B
* Cassette support

## Loading

Load the selected cassette variant through the HC-77B ROM monitor, then start the loaded Advanced Monitor program at its documented load address.

The Advanced Monitor provides its own `]` command prompt. Its included documentation describes the available monitor, editor, assembler, and cassette commands.

## Related Pages

* [Easter 1978 — Advanced Monitor Tools and HC Cassette BASIC](Easter-1978-Advanced-Monitor-and-Cassette-BASIC)
* [HC Cassette BASIC](HC-Cassette-BASIC)
* [HC-77B Technical Overview](HC-77B-Technical-Overview)
