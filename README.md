<p align="center">
<img src="docs/about.png" height=380>
</p>

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
- Followed the [Comet Lake Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#starting-point) to edit the `config.plist`. Used [ProperTree](https://github.com/corpnewt/ProperTree) to edit the plist and [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) to generate the SMBIOS data.
    - In order for audio to work, the `layout-id` of `PciRoot(0x0)/Pci(0x1F,0x3)` must be set to `0B000000` as mentioned [here](https://www.reddit.com/r/hackintosh/comments/i3pega/z490_itx_guide/) and [here](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3).
    - When trying to generate serials for `iMac20,1` (since the Comet Lake guide recommended it for i7-10700K and lower), I found that the `macserial` binary included in GenSMBIOS would not work. To fix this I compiled a new `macserial` binary using the source files found in the [0.6.5 release of OpenCore](https://github.com/acidanthera/OpenCorePkg/releases) with `gcc -std=c99 macserial.c macserial.h -o macserial` and placed this new binary in the `Scripts` directory of GenSMBIOS.
- Used the [sanity checker](https://opencore.slowgeek.com/) and ended up changing a couple values but nothing major.

### BIOS Settings

Followed [Chris Schmock's settings](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3) almost exactly. I ended up updating my BIOS to [1.50](https://www.asrock.com/mb/Intel/Z490%20Phantom%20Gaming-ITXTB3/index.asp#BIOS) using the Windows easy installation process, as I was having troubles with booting into the USB (but it turned out I had put the `EFI` directory in the wrong place, so the update was unnecesasry). I later updated to 1.60 to use a 6900 XT. Since you can only download the 1.60 ROM (and not the Windows easy install program), I copied the `Z49PGIX1.60` file to the same directory as the `ASROM.exe` file (from the 1.50 download) and changed the target ROM file to `Z49PGIX1.60` in `ASROM.ini`.

### Installation

- Unplugged the Linux and Windows drives.
- Booted into the USB, chose `OPENCORE (external)` and formatted the drive to APFS with a GUID partition scheme.
- Installed Catalina. ~~When the computer restarted, I think the `EFI` directory had been deleted from the USB? I dragged it back over from Linux, booted back in and the installation continued.~~ (It turned out that my USB was failing and I was lucky it worked here at all. When installing macOS on another machine I kept getting errors that different kexts or drivers were missing each time I booted from the USB. Tried installing with another USB and it worked perfectly.)

### What worked straight away

**Audio** *(the 3.5 mm rear jack and the USB Razer Blackshark V2 Pro both work perfectly)*, **WiFi**, **Bluetooth**, **AirDrop**, **iMessage**, **Sleep/wake** *(seems to wake with one tap of space bar and one or two clicks. For good measure, disabled "wake for network access" and "power nap" in System Preferences > Energy Saver, and disabled "allow bluetooth devices to wake this computer" in System Preferences > Bluetooth > Advanced)*, **Shutdown**, **Restart**.

### What didn't work

**Ethernet** *(chip showed up in system report but wasn't working, fixed this later on)*.

### Fixing Ethernet

Followed the advice outlined [here](https://www.reddit.com/r/hackintosh/comments/i3pega/z490_itx_guide/) (which was then referenced [here](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3/issues/25) and [here](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3/issues/4)):

> Navigate to System Preferences > Network. Select Ethernet, click Advanced, click Hardware, and Select Configure : Manually with Speed : 1000baseT

### Booting without a USB

Simply followed the [Dortania guide](https://dortania.github.io/OpenCore-Post-Install/universal/oc2hdd.html) again.

### USB Ports

- Mounted the `EFI` partition using [MountEFI](https://github.com/corpnewt/MountEFI).
- Removed `USBInjectAll.kext`.
- Removed `SSDT-EC-USBX-DESKTOP.aml` and added [Schmock's](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3) `SSDT-EC-USBX.aml`.
- (This isn't to do with USB, but added [Schmock's](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3) `SSDT-SBUS-MCHC.aml` as [Papadiche](https://docs.google.com/document/d/1XeUu0YcV2JjsxzpEYQL7mAyqkdN7Q0TTLC6gSsfxzC4/edit) had it too.)
- Added [Papadiche's](https://docs.google.com/document/d/1XeUu0YcV2JjsxzpEYQL7mAyqkdN7Q0TTLC6gSsfxzC4/edit) `USBMap.kext`.
- Added `USBWakeFixup.kext`.
- Opened `config.plist` in [ProperTree](https://github.com/corpnewt/ProperTree), and clicked `File > OC snapshot` to inject the kexts.
- Also changed `Kernel > Quirks > XhciPortLimit` to `False` in the `config.plist`.
- Rebooted.

### Making the boot less verbose

- Mounted the `EFI` partition using [MountEFI](https://github.com/corpnewt/MountEFI).
- Opened `config.plist` in [ProperTree](https://github.com/corpnewt/ProperTree) and:
    - Changed `Misc > Debug > AppleDebug` to `False`.
    - Removed `-v` from `boot-args` in `NVRAM > Add > 7C436110-AB2A-4BBB-A880-FE41995C9F82`.
- Rebooted.

### Using Bootstrap

- Opened `config.plist` in [ProperTree](https://github.com/corpnewt/ProperTree) and:
    - Changed `Misc > Security > BootProtect` to `Bootstrap`.
    - Was told to change `UEFI > Quirks > RequestBootVarRouting` to `True` but it already was.
- Rebooted.

## Updating to macOS Big Sur

- Backed up the `Hackintosh` drive using [Carbon Copy Cloner]().
    - Mounted the `EFI` partitions of `Hackintosh` and the back up drive using [MountEFI](https://github.com/corpnewt/MountEFI).
    - Copied the `EFI` folder from `Hackintosh` over to the back up drive.
    - Booted into the back up drive to check if it was functional.
    - Booted back into the `Hackintosh` drive to continue with the update.
- Updated as usual via System Preferences and all went smoothly.

## Adding Ubuntu boot option to OpenCore

The Windows boot option was automatically available and worked perfectly. Ubuntu seemed to be available too but was called `NO NAME` and I could not boot into it. I logged into Ubuntu and used the disks settings application to rename the `EFI` partition. This changed it from `NO NAME` to `UBUNTU` in the OpenCore boot menu. However, I still could not get this `UBUNTU` option to work like the `Windows` one was by default. So I followed [this guide](https://medium.com/macoclock/guide-multiboot-dualboot-opencore-with-windows-macos-linux-kextcache-131e96784c3f) and (looked at [this guide](https://github.com/sarkrui/Hackintosh-Z390-Aorus-Pro-9700K-RX580/wiki/How-to-add-a-boot-entry-in-OpenCore) too).

- Booted into `OpenShell.efi` and noting down all of the partitions that corresponded to the each operating system.
- Couldn't mount Linux EFI using [MountEFI](https://github.com/corpnewt/MountEFI) so ended up using `distutil` as recommended [here](https://www.insanelymac.com/forum/topic/344324-opencore-last-step-problem-please-help/):
    - Used `distutil list` to list all connected disks.
    - Linux was `disk2` and the `EFI` was partition `1` so I used `sudo distutil mount disk2s1`.
    - You can later unmount with `sudo distutil unmount disk2s1`.
- Ended up with these values:

```
FS0: Linux
PciRoot(0x0)/Pci(0x17,0x0)/Sata(0x0,0xFFFF,0x0)/HD(1,GPT,A1A31A26-6614-44CD-9E03-A145082203FD,0x800,0x100000)/\EFI\ubuntu\grubx64.efi

FS8: Windows
PciRoot(0x0)/Pci(0x1D,0x0)/Pci(0x0,0x0)/NVMe(0x1,B0-29-A6-44-8B-44-1B-00)/\EFI\Microsoft\Boot\bootmgfw.efi
```
- Mounted the Hackintosh `EFI` again and edited the `config.plist`:
    - Added a new entries in `MISC > Entries` with the correct `Path` and `Name` and setting `Enabled` to `True` (one entry for Linux, one entry for Windows).
    - For some reason, the `Windows` entry did not work in the OpenCore menu (it appeared but would not boot, although the original `Windows` was still present and did boot). The new `Linux` entry boots perfectly ~~but the old `UBUNTU` still doesn't work~~. (Deleted the `BOOT` folder present on the Ubuntu drive's `EFI` partition and the `UBUNTU` option disappeared. Note that I had to reinstall the Ubuntu ethernet driver)

### OpenCanopy

Followed the [Dortania OpenCanopy guide](https://dortania.github.io/OpenCore-Post-Install/cosmetic/gui.html#setting-up-opencore-s-gui) and set `MISC > Boot > PickerVariant` to `Default`.
