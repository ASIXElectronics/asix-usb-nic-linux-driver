# ASIX USB to 2.5G/Gigabit/Fast Ethernet Linux driver (`ax_usb_nic`)

Official ASIX USB 3.2 / USB 2.0 to 2.5G/Gigabit/Fast Ethernet Linux driver source for AX88279A, AX88279, AX88179B/A, AX88772E/D, and AX88178A controllers. (Kernel module name: `ax_usb_nic`)

---
## Supported ICs

### USB-to-Ethernet controllers (network driver)

These controllers are bound by the `ax_usb_nic` network driver. All ASIX devices
enumerate under **VID `0x0B95`**; the exact IC is selected by the USB `bcdDevice`
value.

| IC | USB VID:PID | `bcdDevice` | Interface |
|----|-------------|-------------|-----------|
| **AX88279A** | `0B95:1790` | `0x0500` | USB 3.2 to 2.5G Ethernet Chips |
| **AX88279**  | `0B95:1790` | `0x0400` | USB 3.2 to 2.5G Ethernet Chips |
| **AX88179B, AX88179A** | `0B95:1790` | `0x0200` | USB 3.2 to Gigabit Ethernet Chips |
| **AX88179**  | `0B95:1790` | `0x0100` | USB 3.2 to Gigabit Ethernet Chips |
| **AX88178A** | `0B95:178A` | —        | USB 2.0 to Gigabit Ethernet Chips |
| **AX88772E, AX88772D** | `0B95:1790` | `0x0300` | USB 2.0 to Fast Ethernet Chips |

> **AX88179B** and **AX88772E** are newer silicon revisions of AX88179A / AX88772D
> respectively; they enumerate under the same IDs and are handled by the same
> driver. They are also covered by the bundled programming tool (below).

### Bundled flash / eFuse programming tools

The package also ships user-space programming utilities that support:

| Tool | Supported ICs |
|------|---------------|
| `ax88179a_programmer` | AX88179B, AX88179A, AX88772E, AX88772D |
| `ax88279_programmer`  | AX88279A, AX88279 |
| `ax88179_programmer`  | AX88179 |

### Compatible OEM / third-party adapters

The driver also binds the following AX88179-based OEM products:

| Vendor | USB VID:PID |
|--------|-------------|
| Sitecom | `0DF6:0072` |
| Lenovo  | `17EF:304B` |
| Toshiba | `0930:0A13` |
| Samsung | `04E8:A100` |
| D-Link  | `2001:4A00` |
| Magic Control | `0711:0179` |

---

## Prerequisites

- Linux kernel sources installed and matching the running kernel.
- Kernel built with USB host support (XHCI / EHCI / OHCI / UHCI).

## Build

```sh
make
```

On success, `ax_usb_nic.ko` is produced in the current directory.

## Install & load

```sh
sudo make install            # install module into the system
sudo modprobe ax_usb_nic     # load
sudo modprobe -r ax_usb_nic  # unload
modinfo ax_usb_nic           # module info
```

Additional install targets:

```sh
sudo make udev_install       # install udev rule to avoid CDC driver binding
sudo make blacklist_install  # blacklist the in-box ax88179_178a driver (reboot required)
sudo make install_all        # install driver + all configuration files
```

### Manual load

```sh
sudo modprobe mii
sudo insmod ax_usb_nic.ko
sudo rmmod ax_usb_nic
```

---

## License

GPL-2.0. Portions of this driver derive from the Linux kernel; original
copyright notices are retained in the respective source files.

Copyright (c) ASIX Electronics Corporation.
