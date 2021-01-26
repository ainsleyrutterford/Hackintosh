# Hackintosh installation

Installation processes for Windows, Linux, and Mac OS

## Windows 10

All but WiFi and bluetooth worked out of the box. Had to install [this driver](http://en.fenvi.com/en/download_zx.php) (suggested in [this thread](https://www.tonymacx86.com/threads/a-perfect-and-simple-solution-bcm94331-94360-drivers-for-windows-10-8-7.302090/)) for WiFi and Bluetooth to work.

## Ubuntu 20.04.1 LTS

WiFi worked out of the box, ethernet didn't. Installed some updates and then neither worked. Installed [this driver](https://www.realtek.com/en/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-pci-express-software) ("2.5G Ethernet LINUX driver r8125 for kernel up to 5.6" as suggested in the second answer [here](https://askubuntu.com/questions/1259947/cant-get-rtl8125b-working-on-20-04)) and ethernet worked.
