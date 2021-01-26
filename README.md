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

## macOS Catalina

## macOS Big Sur
