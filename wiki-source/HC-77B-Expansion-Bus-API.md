# HC-77B Expansion Bus API

This page documents the public HC-77B extension API for third-party programmers.

The API allows an independently compiled .NET assembly to register one or more HC-77B bus devices with the emulator. A bus device can occupy memory-mapped address ranges, respond to CPU reads and writes, receive reset and tick calls, and assert IRQ or NMI lines.

## Core Concepts

| Concept | Meaning |
|---|---|
| Extension | A loadable assembly entry point implementing `IHc77Extension` |
| Manifest | JSON file listing extension assemblies, types, settings, and enabled state |
| Context | Object supplied to the extension during configuration |
| Bus device | A memory-mapped hardware device registered by an extension |
| Address claim | A declared address range used for validation before attachment |
| Device window | UI-neutral descriptor for a peripheral window exposed to the host UI |

## Extension API Assembly

Third-party extensions are compiled against the public HC-77B extension API assembly supplied with the emulator SDK.

Recommended build reference:

```text
Hc.Emu.HC77.Api.dll
```

The API assembly contains the public contracts required by extensions, including:

- `IHc77Extension`
- `IHc77BusDevice`
- `Hc77ExtensionContext`
- `Hc77AddressClaim`
- `IHc77AddressClaimProvider`
- `Hc77DeviceWindowDescriptor`

Extension authors should reference this DLL at compile time. Extension packages should not ship a private duplicate of the API DLL unless the release notes explicitly say otherwise.

At runtime, the emulator provides the API assembly used to identify and load extension types.

The public SDK package is supplied with the [released developer tools](https://github.com/tmcd35/HomeComp/tree/master/developer-tools/hc77b-extensions-sdk/).

## Loading Model

Extensions are loaded from a manifest file while the HC-77B machine is powered off.

The loader:

1. reads the manifest JSON
2. skips disabled entries
3. resolves each assembly path
4. loads the assembly
5. locates the configured type
6. creates an instance
7. checks that it implements `IHc77Extension`
8. creates an `Hc77ExtensionContext`
9. calls `Configure(context)`
10. validates all registered bus-device address claims
11. attaches the registered bus devices
12. records any registered device windows

If loading fails, the result contains messages explaining the problem.

Extensions are not hot-plugged into a running HC-77B. They must be loaded before power-on.

## Manifest Format

The extension manifest is a JSON file.

It contains a manifest version and a list of extension entries.

```json
{
  "version": 1,
  "extensions": [
    {
      "id": "example-device",
      "enabled": true,
      "assembly": "example-device/ExampleDevice.dll",
      "type": "ExampleDevice.ExampleExtension",
      "settings": {
        "baseAddress": "F400"
      }
    }
  ]
}
```

### Manifest Fields

| Field | Type | Meaning |
|---|---|---|
| `version` | number | Manifest version. Current default is `1`. |
| `extensions` | array | List of extension entries. |

### Extension Entry Fields

| Field | Type | Required | Meaning |
|---|---|---|---|
| `id` | string | Yes | Unique id for this manifest entry |
| `enabled` | bool | No | Whether this extension should be loaded. Defaults to `true` |
| `assembly` | string | Yes | Path to the extension assembly |
| `type` | string | Yes | Fully qualified .NET type implementing `IHc77Extension` |
| `settings` | object | No | String key/value settings passed to the extension |

The loader accepts case-insensitive JSON property names, comments, and trailing commas.

### Assembly Paths

Assembly paths may be absolute or relative.

Relative paths are resolved from the manifest file's directory.

Recommended layout:

```text
extensions/
├── extensions.json
└── example-device/
    ├── ExampleDevice.dll
    └── other-support-files.dll
```

Example manifest path:

```json
{
  "assembly": "example-device/ExampleDevice.dll"
}
```

## Extension Entry Point

Each extension assembly must provide a public type implementing `IHc77Extension`.

```csharp
namespace Hc.Emu.Machines.HC77.Extensions
{
    public interface IHc77Extension
    {
        string ExtensionId { get; }

        string DisplayName { get; }

        void Configure(Hc77ExtensionContext context);
    }
}
```

### `ExtensionId`

A stable programmatic identifier for the extension.

Use lowercase or dotted names that are unlikely to collide:

```text
homecomp.video
thirdparty.example-device
max.rom-board
```

### `DisplayName`

A human-readable name for logs, menus, or future UI display.

Example:

```text
Example Device
HC-77B TTY Interface
```

### `Configure(context)`

The emulator calls `Configure` once during extension loading.

Use it to:

- read extension settings
- create bus device instances
- register bus devices
- register optional device windows

Do not assume the machine is powered on during `Configure`.

## Extension Context

The `Hc77ExtensionContext` is supplied to the extension during configuration.

It contains the extension id, root directory, settings, and registration methods.

| Member | Meaning |
|---|---|
| `ExtensionId` | Id from the manifest entry |
| `ExtensionRootDirectory` | Directory containing the extension assembly |
| `Settings` | String key/value settings from the manifest |
| `RegisterBusDevice(device)` | Registers a memory-mapped bus device |
| `RegisterDeviceWindow(descriptor)` | Registers an optional device window descriptor |

### Settings

Settings are supplied as strings.

Example manifest:

```json
"settings": {
  "baseAddress": "F400",
  "enabledFeature": "true"
}
```

Example use:

```csharp
string configuredBase = context.Settings["baseAddress"];
ushort baseAddress = Convert.ToUInt16(configuredBase, 16);
```

Extensions should validate settings and throw a clear exception if required values are missing or invalid.

## Minimal Extension Example

```csharp
using Hc.Emu.Machines.HC77.Extensions;

namespace ExampleDevice
{
    public sealed class ExampleExtension : IHc77Extension
    {
        public string ExtensionId
        {
            get
            {
                return "example-device";
            }
        }

        public string DisplayName
        {
            get
            {
                return "Example Device";
            }
        }

        public void Configure(Hc77ExtensionContext context)
        {
            ExampleBusDevice device = new();

            context.RegisterBusDevice(device);

            context.RegisterDeviceWindow(
                new Hc77DeviceWindowDescriptor(
                    "example-device.window",
                    "Example Device"));
        }
    }
}
```

## Bus Devices

A bus device is an object registered by an extension so the HC-77B bus can route reads, writes, ticks, resets, and interrupt lines to it.

The public bus-device interface is used by the emulator to perform these operations:

| Member | Purpose |
|---|---|
| `HandlesAddress(ushort address)` | Returns true if the device owns the supplied address |
| `Read(ushort address)` | Returns a byte for a CPU read |
| `Write(ushort address, byte value)` | Handles a CPU write |
| `Tick()` | Advances the device by one emulated machine cycle |
| `Reset()` | Resets the device to its reset state |
| `Irq` | True when the device requests IRQ |
| `Nmi` | True when the device requests NMI |

A device should only return true from `HandlesAddress` for addresses it has claimed.

## Address Claims

Every registered third-party bus device must also implement `IHc77AddressClaimProvider`.

```csharp
namespace Hc.Emu.Machines.HC77.Extensions
{
    public interface IHc77AddressClaimProvider
    {
        IReadOnlyList<Hc77AddressClaim> AddressClaims { get; }
    }
}
```

Address claims are validated before devices are attached.

If a registered bus device does not provide address claims, the extension fails to load.

## `Hc77AddressClaim`

An address claim declares a range owned by a device.

```csharp
public readonly struct Hc77AddressClaim
{
    public Hc77AddressClaim(
        ushort start,
        ushort end,
        string description)

    public ushort Start { get; }
    public ushort End { get; }
    public string Description { get; }

    public bool Contains(ushort address)
    public bool Overlaps(Hc77AddressClaim other)
}
```

### Claim Rules

- `Start` and `End` are inclusive.
- Claims must not overlap built-in machine claims.
- Claims must not overlap already-attached extension claims.
- Claims should describe the function of the range.

Example:

```csharp
private static readonly Hc77AddressClaim[] Claims =
{
    new Hc77AddressClaim(
        0xF400,
        0xF40F,
        "Example Device Registers"),
};

public IReadOnlyList<Hc77AddressClaim> AddressClaims
{
    get
    {
        return Claims;
    }
}
```

## Built-In Address Claims

The loader automatically protects built-in machine areas from extension overlap.

| Range | Claim |
|---|---|
| `$0000-$03FF` | Base RAM |
| `$F800-$FFFF` | Monitor ROM |
| `$F730` | Cassette data |
| `$F731` | Cassette status |
| `$F732` | Cassette control |
| `$F710-$F712` | Front panel keypad |
| `$F720-$F729` | Front panel display |

If an extension claims any of these ranges, it will fail to load.

## HC-77B Address Regions

The HC-77B exposes the following high-level address regions.

| Range | Region | Extension Guidance |
|---|---|---|
| `$0000-$03FF` | Base RAM | Built in; do not claim |
| `$0400-$0FFF` | RAM expansion | For RAM expansion devices |
| `$1000-$EFFF` | General expansion | Suitable for expansion RAM, ROM, and mapped devices |
| `$F000-$F3FF` | Video region | For video-related devices |
| `$F400-$F6FF` | Device/peripheral space | Preferred peripheral I/O area |
| `$F700-$F7FF` | Base I/O | Mostly reserved for core machine I/O |
| `$F800-$FFFF` | Monitor ROM | Built in; do not claim |

Third-party extensions should prefer `$F400-$F6FF` for new peripheral registers unless a released device specification says otherwise.

## Bus Routing Order

When the CPU reads an address, the HC-77B bus checks targets in this order:

1. base RAM
2. cassette read addresses
3. front-panel addresses
4. attached extension bus devices
5. monitor ROM
6. open bus value

When the CPU writes an address, the bus checks targets in this order:

1. base RAM
2. cassette write addresses
3. front-panel addresses
4. attached extension bus devices
5. monitor ROM write ignore

This means built-in base RAM, cassette, front panel, and ROM behaviour cannot be overridden by extensions.

## Open Bus

If no device handles a read, the bus returns the emulator's default open-bus value.

Extensions should not rely on unclaimed addresses returning a specific value unless that behaviour is part of the released machine specification.

## Read and Write Behaviour

A bus device should implement reads and writes as hardware register access.

Recommended pattern:

```csharp
public bool HandlesAddress(ushort address)
{
    return address >= 0xF400 && address <= 0xF40F;
}

public byte Read(ushort address)
{
    switch (address)
    {
        case 0xF400:
            return _status;

        case 0xF401:
            return _data;

        default:
            return 0xFF;
    }
}

public void Write(ushort address, byte value)
{
    switch (address)
    {
        case 0xF401:
            _data = value;
            break;

        case 0xF402:
            _control = value;
            break;
    }
}
```

A device should ignore writes to read-only registers unless its own specification says otherwise.

## Tick Behaviour

The bus calls `Tick()` on attached extension devices once per emulated machine cycle.

The HC-77B CPU clock is:

```text
1,000,000 Hz
```

A device that needs timing should count ticks.

Example:

```csharp
private int _divider;

public void Tick()
{
    _divider = _divider + 1;

    if (_divider >= 1000)
    {
        _divider = 0;
        ServiceSlowDevice();
    }
}
```

Keep `Tick()` efficient. It may be called very frequently.

## Reset Behaviour

The bus calls `Reset()` on attached devices when the HC-77B is reset or powered off.

A device should reset volatile state in `Reset()`.

Example:

```csharp
public void Reset()
{
    _status = 0;
    _data = 0;
    _control = 0;
    _irq = false;
    _nmi = false;
}
```

Persistent host-side settings should not normally be reset by the emulated machine reset.

## Interrupt Lines

Each bus device exposes IRQ and NMI request state.

The HC-77B bus aggregates these lines:

- if any attached device has `Irq == true`, the bus IRQ line is active
- if any attached device has `Nmi == true`, the bus NMI line is active

A device should clear its interrupt request when the emulated software performs the documented acknowledgement action.

Example:

```csharp
public bool Irq
{
    get
    {
        return _irq;
    }
}

public bool Nmi
{
    get
    {
        return false;
    }
}
```

Only use NMI for devices that genuinely require non-maskable interrupt behaviour.

## Device Windows

An extension may register one or more device window descriptors.

```csharp
public sealed class Hc77DeviceWindowDescriptor
{
    public Hc77DeviceWindowDescriptor(
        string deviceId,
        string displayName)

    public string DeviceId { get; }
    public string DisplayName { get; }
}
```

A descriptor is UI-neutral. It tells the emulator host that the extension has a device window concept, but it does not itself define a UI toolkit or rendering surface.

Example:

```csharp
context.RegisterDeviceWindow(
    new Hc77DeviceWindowDescriptor(
        "example-device.panel",
        "Example Device"));
```

### Device Window Guidance

Use stable `deviceId` values.

Good examples:

```text
tty-interface.window
video-display.window
keyboard.window
example-device.panel
```

The `displayName` should be short and suitable for a Devices menu.

## Complete Example Device

```csharp
using System.Collections.Generic;
using Hc.Emu.Machines.HC77.Bus;
using Hc.Emu.Machines.HC77.Extensions;

namespace ExampleDevice
{
    public sealed class ExampleExtension : IHc77Extension
    {
        public string ExtensionId
        {
            get
            {
                return "example-device";
            }
        }

        public string DisplayName
        {
            get
            {
                return "Example Device";
            }
        }

        public void Configure(Hc77ExtensionContext context)
        {
            ExampleBusDevice device = new();

            context.RegisterBusDevice(device);

            context.RegisterDeviceWindow(
                new Hc77DeviceWindowDescriptor(
                    "example-device.window",
                    "Example Device"));
        }
    }

    public sealed class ExampleBusDevice :
        IHc77BusDevice,
        IHc77AddressClaimProvider
    {
        private byte _status;
        private byte _data;
        private bool _irq;

        private static readonly Hc77AddressClaim[] Claims =
        {
            new Hc77AddressClaim(
                0xF400,
                0xF40F,
                "Example Device Registers"),
        };

        public IReadOnlyList<Hc77AddressClaim> AddressClaims
        {
            get
            {
                return Claims;
            }
        }

        public bool Irq
        {
            get
            {
                return _irq;
            }
        }

        public bool Nmi
        {
            get
            {
                return false;
            }
        }

        public bool HandlesAddress(ushort address)
        {
            return address >= 0xF400 && address <= 0xF40F;
        }

        public byte Read(ushort address)
        {
            switch (address)
            {
                case 0xF400:
                    return _status;

                case 0xF401:
                    _irq = false;
                    return _data;

                default:
                    return 0xFF;
            }
        }

        public void Write(ushort address, byte value)
        {
            switch (address)
            {
                case 0xF401:
                    _data = value;
                    _status = 1;
                    _irq = true;
                    break;

                case 0xF402:
                    _status = value;
                    break;
            }
        }

        public void Tick()
        {
        }

        public void Reset()
        {
            _status = 0;
            _data = 0;
            _irq = false;
        }
    }
}
```

## Complete Manifest Example

```json
{
  "version": 1,
  "extensions": [
    {
      "id": "example-device",
      "enabled": true,
      "assembly": "example-device/ExampleDevice.dll",
      "type": "ExampleDevice.ExampleExtension",
      "settings": {
        "baseAddress": "F400"
      }
    }
  ]
}
```

## Error Handling

The loader reports errors through `Hc77ExtensionLoadResult`.

| Condition | Result |
|---|---|
| Missing manifest path | Message: no manifest path supplied |
| Manifest file absent | Message: no manifest found |
| Manifest cannot be read | Error |
| Enabled entry has no id | Error |
| Enabled entry has no assembly path | Error |
| Enabled entry has no type | Error |
| Assembly file missing | Error |
| Type not found | Error |
| Type does not implement `IHc77Extension` | Error |
| Registered bus device has no address claims | Error |
| Address claim overlaps existing claim | Error |
| Extension throws during load | Error with exception message |

A failed extension is not attached.

## Extension Load Result

`Hc77ExtensionLoadResult` contains:

| Member | Meaning |
|---|---|
| `Success` | False if any error was recorded |
| `LoadedExtensionIds` | Ids of successfully loaded extensions |
| `Messages` | Loader messages and errors |
| `DeviceWindows` | Device windows registered by loaded extensions |

## Power and Attachment Rules

Extensions are loaded while the machine is off.

At machine level:

- `LoadExtensions(manifestPath)` fails if the HC-77B is powered on
- direct device attachment is ignored while powered on
- devices receive reset calls during machine reset/power-off
- devices receive tick calls only while the machine is running

This preserves the model of peripherals being attached before use.

## Compatibility Guidance

Third-party extensions should:

- use documented public interfaces only
- declare address claims for every registered bus device
- avoid built-in machine address ranges
- keep `Tick()` efficient
- avoid assuming extension load order unless documented by the manifest
- treat settings as optional/untrusted input
- expose stable extension and device ids
- keep host UI assumptions out of bus-device logic
- document every register they expose

## Naming Guidance

Recommended extension ids:

```text
author.device-name
homecomp.device-name
thirdparty.device-name
```

Recommended device ids:

```text
device-name.window
device-name.panel
device-name.display
```

Recommended assembly layout:

```text
extensions/
└── my-device/
    ├── MyDevice.dll
    ├── MyDevice.deps.json
    └── support-files/
```

## Register Documentation Template

Every extension should document its registers.

Example:

| Address | Name | R/W | Meaning |
|---|---|---|---|
| `$F400` | STATUS | R | Device status |
| `$F401` | DATA | R/W | Data register |
| `$F402` | CONTROL | W | Control register |

For each register, describe:

- reset value
- read behaviour
- write behaviour
- side effects
- interrupt behaviour
- timing assumptions

## API Stability

The HC-77B expansion API is intended to be the stable public interface for HC-77B third-party extensions.

Future HomeComp machines may have their own APIs or additional extension points. HC-77B extensions should not assume binary compatibility with later machines unless a later release explicitly provides it.
