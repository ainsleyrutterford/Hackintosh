# Hackintosh installation

Notes about the installation processes for Windows, Linux, and macOS on my new build.

## Hardware

## Installing Windows 10

Created installation USB using [this tool](https://www.microsoft.com/en-gb/software-download/windows10ISO) and installed with only one drive installed.

All but WiFi and bluetooth worked out of the box. Had to install [this driver](http://en.fenvi.com/en/download_zx.php) (suggested in [this thread](https://www.tonymacx86.com/threads/a-perfect-and-simple-solution-bcm94331-94360-drivers-for-windows-10-8-7.302090/)) for WiFi and Bluetooth to work.

## Installing Ubuntu 20.04.1 LTS

Created installation USB by following [this guide](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview) which uses [Rufus](https://rufus.ie/). Unplugged Windows drive and plugged in Linux drive and installed as usual.

WiFi worked out of the box, ethernet didn't. Installed some updates and then neither worked. Installed [this driver](https://www.realtek.com/en/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-pci-express-software) ("2.5G Ethernet LINUX driver r8125 for kernel up to 5.6" as suggested in the second answer [here](https://askubuntu.com/questions/1259947/cant-get-rtl8125b-working-on-20-04)) and ethernet worked.

To get WiFi working again, installed `bcmwl-kernel-source` from `groovy` repos by following the first step in the first answer [here](https://askubuntu.com/questions/1305699/bcmwl-kernel-source-broken-on-kernel-5-8-0-34-generic). You need to download [this file](http://mirrors.kernel.org/ubuntu/pool/restricted/b/bcmwl/bcmwl-kernel-source_6.30.223.271+bdcom-0ubuntu7_amd64.deb) and install it via:

```
sudo dpkg -i bcmwl-kernel-source_6.30.223.271+bdcom-0ubuntu7_amd64.deb
```

Once this was installed, WiFi and bluetooth worked perfectly again.

## Installing macOS Catalina

Ended up doing most of this process on Ubuntu 20.04.1 instead of on my MacBook Pro. Followed the [Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/) and used this [YouTube video](https://www.youtube.com/watch?v=eUnVzJsINCI) for an idea of what to do aswell.

<!-- THIS WAS ALL FOLLOWING THE SCHMOCK GUIDE AND I REDID IT ALL ON LINUX INSTEAD OF MAC:

### Creating the installation USB

- Downloaded the "Install macOS Catalina" application via [this link](https://go.redirectingat.com/?id=803X112722&xcust=41-3629363-11-0000000&sref=https%3A%2F%2Fwww.macworld.co.uk%2Fhow-to%2Fdownload-old-os-x-3629363%2F&xs=1&url=https%3A%2F%2Fapps.apple.com%2Fsg%2Fapp%2Fmacos-catalina%2Fid1466841314%3Fmt%3D12) (open it in Safari on a Mac).
- Used Disk Utility to format the installation USB to Mac OS Extended (Journaled) with a GUID Partition Scheme.
- Finally, typed `sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume` in the terminal to create the installer.

### Creating an EFI folder

- Mounted the EFI partition of "Install macOS Catalina" using [Hackintool](https://github.com/headkaze/Hackintool).
- Downloaded a premade EFI folder from [Chris Schmock's repo](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3) and copied it to the EFI partition.
- Renamed `config_iMac20,2_iGPU_computing_only.plist` to `config.plist` (inside `EFI/OC`).
- Followed [this guide](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html#generate-a-new-serial) to generate new `MLB`, `SystemSerialNumber`, and `SystemUUID` serial numbers. I used [GenSMBOIS](https://github.com/corpnewt/GenSMBIOS) and used a serial that wasn't "valid", since the guide (and every answer I found online) said that this normally works.
- Used [PlistEdit Pro](https://www.fatcatsoftware.com/plisteditpro/) to paste the serials into the relevant fields in `Root > PlatformInfo > Generic` in `config.plist`. -->

### Creating the installation USB

- Followed the Dortania guide and used [macrecovery.py](https://github.com/acidanthera/OpenCorePkg/releases) to chose Catelina 10.15.7.
- The firmware drivers and kexts I chose are available in the [EFI](EFI) directory.
- I think I used a prebuilt SSDT-AWAC but created the rest myself using [SSDTTime](https://github.com/corpnewt/SSDTTime).
- Followed the [Comet Lake Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#starting-point) to edit the `config.plist`. Used [ProperTree](https://github.com/corpnewt/ProperTree) to edit the plist and [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) to generate the SMBIOS data.
    - In order for audio to work, the `layout-id` of `PciRoot(0x0)/Pci(0x1F,0x3)` must be set to `0B000000` as mentioned [here](https://www.reddit.com/r/hackintosh/comments/i3pega/z490_itx_guide/) and [here](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3).
    - When trying to generate serials for `iMac20,1` (since the Comet Lake guide recommended it for i7-10700K and lower), I found that the `macserial` binary included in GenSMBIOS would not work. To fix this I compiled a new `macserial` binary using the source files found in the [0.6.5 release of OpenCore](https://github.com/acidanthera/OpenCorePkg/releases) with `gcc -std=c99 macserial.c macserial.h -o macserial` and placed this new binary in the `Scripts` directory of GenSMBIOS.
- Used the [sanity checker](https://opencore.slowgeek.com/) and ended up changing a couple values but nothing major.

### BIOS Settings

Followed [Chris Schmock's settings](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3) almost exactly. I ended up updating my BIOS to [1.50](https://www.asrock.com/mb/Intel/Z490%20Phantom%20Gaming-ITXTB3/index.asp#BIOS) using the Windows easy installation process, as I was having troubles with booting into the USB (but it turned out I had put the `EFI` directory in the wrong place, so the update was unnecesasry).

### Installation

- Unplugged the Linux and Windows drives.
- Booted into the USB, chose `OPENCORE (external)` and formatted the drive to APFS with a GUID partition scheme.
- Installed Catalina. When the computer restarted, I think the `EFI` directory had been deleted from the USB? I dragged it back over from Linux, booted back in and the installation continued.

### What worked straight away

- **Audio:** the 3.5 mm rear jack and the USB Razer Blackshark V2 Pro both work perfectly.
- **WiFi**
- **Bluetooth**
- **AirDrop**
- **iMessage**
- **Sleep/wake:** seems to wake with one tap of space bar and one or two clicks. For good measure, disabled "wake for network access" and "power nap" in System Preferences > Energy Saver, and disabled "allow bluetooth devices to wake this computer" in System Preferences > Bluetooth > Advanced.
- **Shutdown**
- **Restart**

### What didn't work

- **Ethernet:** chip showed up in system report but wasn't working (FIXED).
- **USB Ports:** although the four I was using all worked perfectly 95% of the time, the keyboard did stop working at one point and only continued to work after changing USB port, so I decided to ... USB TABLE?

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
- Rebooted, and ...

### Making the boot less verbose

Followed Dortania guide and Papadiche's guide.
- Mounted the `EFI` partition using [MountEFI](https://github.com/corpnewt/MountEFI).

## Updating to macOS Big Sur

Updated as usual? Looked at these discussions...

## Triple Boot

Followed this guide.

### OpenCanopy

Followed Dortania guide and this guide.