<div>

![About dark](./docs/about-dark.png#gh-dark-mode-only)

</div>

<div>

![About light](./docs/about-light.png#gh-light-mode-only)

</div>

# Hackintosh

Notes about the installation processes for Windows, and macOS on an ASRock Z490 Phantom Gaming-ITX/TB3. Some benchmark scores are available [here](docs/benchmarks.md).

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
- **Power Supply:** Corsair SF600

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

## What doesn't work

- Nothing that I am aware of

## Installing Windows 10

Created installation USB using [this tool](https://www.microsoft.com/en-gb/software-download/windows10ISO) and installed with only one drive installed.

All but WiFi and bluetooth worked out of the box. Had to install [this driver](http://en.fenvi.com/en/download_zx.php) (suggested in [this thread](https://www.tonymacx86.com/threads/a-perfect-and-simple-solution-bcm94331-94360-drivers-for-windows-10-8-7.302090/)) for WiFi and Bluetooth to work.

## Installing macOS Monterey

Followed the [Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/) on Windows 10. Created a USP map in Windows using [USBToolBox](https://github.com/USBToolBox/tool) before installing Monterey.

### Creating the installation USB

- The firmware drivers and kexts I chose are available in the [EFI](EFI) directory.
- Used a prebuilt SSDT-EC-USBX but created the rest myself using [SSDTTime](https://github.com/corpnewt/SSDTTime).
- Followed the [Comet Lake Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#starting-point) to edit the `config.plist`.
- In order for audio to work, the `layout-id` of `PciRoot(0x0)/Pci(0x1F,0x3)` must be set to `0B000000` as mentioned [here](https://www.reddit.com/r/hackintosh/comments/i3pega/z490_itx_guide/) and [here](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3).

### BIOS Settings

Updated the BIOS to [1.60](https://www.asrock.com/mb/Intel/Z490%20Phantom%20Gaming-ITXTB3/index.asp#BIOS) using the Windows easy installation process to enable the use of a 6900 XT. Since you can only download the 1.60 ROM (and not the Windows easy install program), I copied the `Z49PGIX1.60` file to the same directory as the `ASROM.exe` file (from the 1.50 download) and changed the target ROM file to `Z49PGIX1.60` in `ASROM.ini`.

### Installation

- Unplugged the Windows drive.
- Booted into the USB, chose `OPENCORE (external)` and formatted the drive to APFS with a GUID partition scheme.
- Installed Monterey.

### What worked straight away

**Audio** *(the 3.5 mm rear jack and the USB Razer Blackshark V2 Pro both work perfectly)*, **WiFi**, **Bluetooth**, **AirDrop**, **iMessage**, **Sleep/wake** *(seems to wake with one tap of space bar and one or two clicks. For good measure, disabled "wake for network access" and "power nap" in System Preferences > Energy Saver, and disabled "allow bluetooth devices to wake this computer" in System Preferences > Bluetooth > Advanced)*, **Shutdown**, **Restart**.

### What didn't work

**Ethernet** *(chip showed up in system report but wasn't working, fixed this later on)*.

## Post Installation

### Fixing Ethernet

Followed the advice outlined [here](https://www.reddit.com/r/hackintosh/comments/i3pega/z490_itx_guide/) (which was then referenced [here](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3/issues/25) and [here](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3/issues/4)):

> Navigate to System Preferences > Network. Select Ethernet, click Advanced, click Hardware, and Select Configure : Manually with Speed : 1000baseT

### Using LauncherOption

Creates an OpenCore boot option in the BIOS and stops Windows update from messing things up.

Followed the [LauncherOption page](https://dortania.github.io/OpenCore-Post-Install/multiboot/bootstrap.html#using-launcheroption) in the Dortania post-install guide.
