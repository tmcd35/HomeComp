# Emulator User Manual

This page explains how to run the released HomeComp emulator as a modern application.

It is not the HC-77B machine user manual. The fictional HC-77B manual is included separately in PDF form with the emulator release and explains the front-panel monitor, address entry, data entry, register display, program execution, and cassette operations.

Release files are stored in the main [HomeComp repository](https://github.com/tmcd35/HomeComp).

## Current Release Scope

The current public release covers the HomeComp's HC-77B Single Board Computer.

September 1977 saw the release of the HC-77B system itself along with optional expansion hardware to connect a Teletype (TTY) device, Keyboard, and television display controller.

In November 1977 a 3K RAM Expansion was released, giving a total 4K user space. The RAM expansion was bundled with a free cassette game, NIM.

Easter 1978 brought the first two major cassette software releases: an Advanced Monitor Tools cassette giving command-line access to the system ROM, a text editor, and a 6502 assembler; and HC Cassette BASIC.

Mid 1978 adds the HC Scrolling LED Display: a 32×8, single-colour red LED peripheral and a base-machine demonstration cassette. The display appears through the emulator's **Devices** menu when its extension is installed.

September 1978 adds the HC-77B 8K ROM BASIC configuration. The ROM BASIC release provides a resident programming environment for TTY and Keyboard/CRT systems and includes compatibility import for earlier HC Cassette BASIC programs.

Easter 1979 introduces the HC-77B Complete System Bundle, bringing the mature 4K RAM, Keyboard/CRT and ROM BASIC configuration together as a complete system.

Late 1979 adds the **HC-77B Text & Printer Pack**. The expansion provides a 40-column virtual impact printer together with the resident Text Editor and printer ROM. The printer appears through the emulator's **Devices** menu and supports persistent virtual paper, manual paper feed, PDF export, host printing and clearing of printed output.

## Supported Platforms

The emulator is currently released for:

| Platform | Build |
|---|---|
| Windows | win-x64 |
| macOS | macOS-x64 |
| macOS Apple Silicon | macOS-arm64 |
| Linux | Linux |

## Application Names

The emulator executable/application name depends on platform:

| Platform | Application |
|---|---|
| Windows | `HC-77B.exe` |
| macOS | `HC-77B Emulator.app` |
| Linux | `HC-77B` |

## Release Folder Layout

The emulator release uses the following folder structure:

```text
config/
docs/
extensions/
roms/
tapes/
```

### `config/`

Stores emulator configuration files.

This folder is used for host-side emulator settings rather than fictional HC-77B machine files.

### `docs/`

Contains included documentation.

This is where the HC-77B machine manual and other public documentation files are expected to live.

### `extensions/`

Contains released emulator extensions and peripheral examples.

### `roms/`

Contains ROM files used by the emulator.

The HC-77B boot ROM is included with the release.

### `tapes/`

Contains cassette image files used by the emulator.


## Launching the Emulator

### Windows

Run:

```text
HC-77B.exe
```

### macOS

Open:

```text
HC-77B Emulator.app
```

On macOS, the first launch may be blocked because the app is not notarised. Open **System Settings → Privacy & Security**, find the message about `HC-77B Emulator.app`, then choose **Open Anyway**.

If macOS blocks the app even after using **Open Anyway**, remove the quarantine attribute from Terminal:

```bash
xattr -dr com.apple.quarantine "HC-77B Emulator.app"
```

### Linux

Run:

```text
HC-77B
```

If the file is not executable, set executable permission from a terminal:

```bash
chmod +x HC-77B
```

Then launch it again.

## Emulator Menus

The emulator provides three main menus:

```text
System
Devices
About
```

![HC-77B emulator System menu](https://github.com/tmcd35/HomeComp/raw/master/site-assets/images/wiki/hc-77b-system-menu.png)

*The emulator System menu provides the main power and application controls.*

### System

The System menu provides Power On and Power Off controls for starting and stopping the emulated hardware, plus Exit to quit the application.

### Devices

The Devices menu is used to access attached or available emulator devices.

Devices may include built-in emulator windows, released peripherals, or future extension-provided hardware. The exact contents of this menu depend on the current release package and installed extensions.

### About

The About menu provides emulator and project information.

## Using the HC-77B Machine

The HC-77B is a front-panel-first 6502 microcomputer.

Machine-level operation is explained in the included HC-77B User Manual PDF. Use that manual for:

- switching on the fictional machine
- understanding the front-panel display
- using the hexadecimal keypad
- entering addresses
- entering data
- inspecting memory
- viewing registers
- starting programs with `GO`
- using cassette `LOAD` and `SAVE`
- following worked example programs

The emulator presents the machine. The included manual explains how to operate that machine.

## Boot ROM

The HC-77B boot ROM is included with the emulator release.

Users do not need to supply a separate boot ROM for the September 1977 launch release.

## Cassette Media

Cassette images belong in:

```text
tapes/
```


## Extensions

Extension files belong in:

```text
extensions/
```

The expansion and peripheral API is documented separately in the Expansion Bus API section of this wiki.

## Troubleshooting

### The emulator does not launch

Check that you are using the correct build for your platform:

| Platform | Expected file |
|---|---|
| Windows | `HC-77B.exe` |
| macOS | `HC-77B Emulator.app` |
| Linux | `HC-77B` |

Also check that the release folder has not been partially extracted or rearranged.

### Linux build will not run

Make sure the emulator has executable permission:

```bash
chmod +x HC-77B
```

Then run it again.

### macOS blocks the application

macOS may warn about downloaded applications that are not distributed through the App Store.

Use the normal macOS security process for opening a trusted downloaded app.

### The emulator reports missing files

Check that the release folders are still present:

```text
config/
docs/
extensions/
roms/
tapes/
```

The boot ROM is included with the release, but the emulator still expects its normal folder structure to be present.

### A downloaded file looks too small or contains Git LFS text

If a downloaded file opens as text beginning with something like:

```text
version https://git-lfs.github.com/spec/v1
```

then the Git LFS pointer was downloaded instead of the actual file.

Use GitHub’s proper download controls or clone the repository with Git LFS support enabled.

## Version Notes

This page describes the emulator user-facing behaviour for the September 1977 HC-77B public release.

The wiki will expand as later HomeComp ecosystem releases add new machines, software, cassette media, peripherals, and emulator features.
