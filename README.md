<div align="center">

![About dark](./docs/about-dark.png#gh-dark-mode-only)
![About light](./docs/about-light.png#gh-light-mode-only)

</div>

# Hackintosh

Notes about the installation processes for Windows 10 and macOS Monterey on an ASRock Z490 Phantom Gaming-ITX/TB3. Some benchmark scores are available [here](docs/benchmarks.md).

## Hardware

- **Case:** NCASE M1 V5.0
- **CPU:** Intel i7-10900K
- **GPU:** Gigabyte RX 6900 XT Gaming OC 
- **Memory:** Corsair Vengeance LPX DDR4 3000MHz 32GB
- **Motherboard:** ASRock Z490 Phantom Gaming-ITX/TB3
    - **Audio card:** Realtek ALC1220-VB
    - **WiFi/Bluetooth card:** Broadcom BCM94360NG
    - **Ethernet card:** Realtek RTL8125B-CG
- **Storage:**
    - WD BLACK SN750 NVMe 1TB — macOS Monterey
    - WD BLACK SN750 NVMe 1TB (with heatsink) — Windows 10
    - Kingston 500GB SSD — Ubuntu 20.04.1 LTS
    - Kingston 2TB SSD — Shared internal storage
- **Power Supply:** Corsair SF750

## What works

- **Audio**
- **Ethernet**
- **WiFi**
- **Bluetooth**
- **AirDrop**
- **iMessage**
- **iCloud**
- **Find My**
- **Sleep/wake**
- **Shutdown**
- **Restart**

What doesn't work? Nothing that I am aware of!

## Installing Windows 10

Created installation USB using [this tool](https://www.microsoft.com/en-gb/software-download/windows10ISO) and installed with only one drive installed.

All but WiFi and bluetooth worked out of the box. Had to install [this driver](http://en.fenvi.com/en/download_zx.php) (suggested in [this thread](https://www.tonymacx86.com/threads/a-perfect-and-simple-solution-bcm94331-94360-drivers-for-windows-10-8-7.302090/)) for WiFi and Bluetooth to work.

## Installing macOS Monterey

Followed the [Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/) on Windows 10. In order to avoid using the USBInjectAll kext and `XhciPortLimit`, I added [Papadiche's USBMap kext](https://drive.google.com/file/d/1kAo5eO-IT8NQvanriptEmJqI_Kbyh4gb/view) (from his [guide](https://docs.google.com/document/d/1XeUu0YcV2JjsxzpEYQL7mAyqkdN7Q0TTLC6gSsfxzC4/edit)) before installing Monterey.

### Creating the installation USB

Followed the [Dortania Comet Lake guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#starting-point) to edit the `config.plist`:
- The firmware drivers and kexts I chose are available in the [EFI](EFI) directory.
- Used a prebuilt `SSDT-EC-USBX.aml` from the Comet lake guide, created `SSDT-AWAC.aml` and `SSDT-PLUG.aml` myself using [SSDTTime](https://github.com/corpnewt/SSDTTime), and downloaded an `SSDT-SBUS-MCHC.aml` that [Schmock built](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3).
- In order for audio to work, the `layout-id` of `PciRoot(0x0)/Pci(0x1F,0x3)` must be set to `0B000000` as mentioned [here](https://www.reddit.com/r/hackintosh/comments/i3pega/z490_itx_guide/) and [here](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3).

### BIOS Settings

Updated the BIOS to [1.60](https://www.asrock.com/mb/Intel/Z490%20Phantom%20Gaming-ITXTB3/index.asp#BIOS) using the Windows easy installation process to enable the use of a 6900 XT. Since you can only download the 1.60 ROM (and not the Windows easy install program), I copied the `Z49PGIX1.60` file to the same directory as the `ASROM.exe` file (from the 1.50 download) and changed the target ROM file to `Z49PGIX1.60` in `ASROM.ini`.

### Installation

- Unplugged the Windows drive.
- Booted into the USB, chose `OPENCORE (external)` and formatted the drive to APFS with a GUID partition scheme.
- Installed Monterey.

## Post Installation

### Using LauncherOption

This is important for multi booting Windows and macOS as it creates an OpenCore boot option in the BIOS and stops Windows update from messing things up. Followed the [LauncherOption page](https://dortania.github.io/OpenCore-Post-Install/multiboot/bootstrap.html#using-launcheroption) in the Dortania post-install guide.

### Fixing slow wake from sleep

The [USBWakeFixup kext](https://github.com/osy/USBWakeFixup) was necessary to enable wake from sleep in under 3 seconds. I also used Papadiche's USB Map instead of one I created myself using [USBToolBox](https://github.com/USBToolBox/tool) on Windows.

## Useful macOS applications

- [MonitorControl](https://github.com/MonitorControl/MonitorControl) enables changing the brightness of external monitors.
- [SteerMouse](https://plentycom.jp/en/steermouse/) (free alternatives: [LinearMouse](https://github.com/linearmouse/linearmouse) and [Mac Mouse Fix](https://mousefix.org/)) can be used to remove mouse acceleration.

## Acknowledgements

- [The Dortania OpenCore guide](https://dortania.github.io/OpenCore-Install-Guide) is unbelievably useful and complete.
- [Papadiche's guide](https://docs.google.com/document/d/1XeUu0YcV2JjsxzpEYQL7mAyqkdN7Q0TTLC6gSsfxzC4/edit) helped fix the on-board audio.
- [Chris Schmock's build](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3) was used for the `SSDT-SBUS-MCHC.aml`.