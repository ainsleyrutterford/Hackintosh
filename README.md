# Hackintosh installation

Installation processes for Windows, Linux, and Mac OS

## Windows 10

Created installation USB using [this tool](https://www.microsoft.com/en-gb/software-download/windows10ISO) and installed with only one drive installed.

All but WiFi and bluetooth worked out of the box. Had to install [this driver](http://en.fenvi.com/en/download_zx.php) (suggested in [this thread](https://www.tonymacx86.com/threads/a-perfect-and-simple-solution-bcm94331-94360-drivers-for-windows-10-8-7.302090/)) for WiFi and Bluetooth to work.

## Ubuntu 20.04.1 LTS

Created installation USB by following [this guide](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview) which uses [Rufus](https://rufus.ie/). Unplugged Windows drive and plugged in Linux drive and installed as usual.

WiFi worked out of the box, ethernet didn't. Installed some updates and then neither worked. Installed [this driver](https://www.realtek.com/en/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-pci-express-software) ("2.5G Ethernet LINUX driver r8125 for kernel up to 5.6" as suggested in the second answer [here](https://askubuntu.com/questions/1259947/cant-get-rtl8125b-working-on-20-04)) and ethernet worked.

To get WiFi working again, installed `bcmwl-kernel-source` from `groovy` repos by following the first step in the first answer [here](https://askubuntu.com/questions/1305699/bcmwl-kernel-source-broken-on-kernel-5-8-0-34-generic). You need to download [this file](http://mirrors.kernel.org/ubuntu/pool/restricted/b/bcmwl/bcmwl-kernel-source_6.30.223.271+bdcom-0ubuntu7_amd64.deb) and install it via

```
sudo dpkg -i bcmwl-kernel-source_6.30.223.271+bdcom-0ubuntu7_amd64.deb
```

Once this was installed, WiFi and bluetooth worked perfectly again.

## macOS Catalina

### Creating the installation USB

- Downloaded the Install macOS Catalina application via [this link](https://go.redirectingat.com/?id=803X112722&xcust=41-3629363-11-0000000&sref=https%3A%2F%2Fwww.macworld.co.uk%2Fhow-to%2Fdownload-old-os-x-3629363%2F&xs=1&url=https%3A%2F%2Fapps.apple.com%2Fsg%2Fapp%2Fmacos-catalina%2Fid1466841314%3Fmt%3D12) (open it in Safari on a Mac).
- Used Disk Utility to format the installation USB to Mac OS Extended (Journaled) with a GUID Partition Scheme.
- Finally, typed `sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume` in the terminal to create the installer.

### Creating an EFI folder

- Downloaded a premade EFI folder from [Chris Schmock's repo](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3).
- Renamed the `config_iMac20,2_iGPU_computing_only.plist` to `config.plist`.
- Followed [this guide](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html#generate-a-new-serial) to generate new `MLB`, `SystemSerialNumber`, and `SystemUUID` serial numbers. I used [GenSMBOIS](https://github.com/corpnewt/GenSMBIOS) and used a serial that wasn't "valid", since the guide (and every answer I found online) said that this normally works.
- Pasted the serials into the relevant fields in `Root > PlatformInfo > Generic` in the `config.plist`.

## macOS Big Sur
