# usb-c-gadget

I’ve previously [blogged](https://blog.hardill.me.uk/2017/01/23/raspberry-pi-zero-gadgets/) about using [Pi Zero](https://blog.hardill.me.uk/2017/02/12/updated-pi-zero-gadgets/)(and Zero W) devices as USB Gadgets. This allows them to be powered and accessed via one of the micro USB sockets and it shows up as both a CD-Drive and a ethernet device.

A recent update to the Raspberry Pi 4 bootloader not only enables the low power mode for the USB hardware, allows the enabling of Network boot and enables data over the USB-C port. The lower power means it should run (without any hats) with the power supplied from a laptop.

Details of how to check/update the bootloader can be found [here](https://www.raspberrypi.org/documentation/hardware/raspberrypi/booteeprom.md).

Given that the Pi4 has a Gigabit Ethernet adapter, WiFi and 4 USB sockets (need to keep the power draw low to be safe) and up to 4Gb RAM to go with it’s 4 x 1.5Ghz core processor it makes for a very attractive plugin compute device.

With this enabled all the same script from the Pi Zero’s should just work but here is the updated version for Raspbian Buster.

- Add `dtoverlay=dwc2` to the `/boot/config.txt`
- Add `modules-load=dwc2` to the end of `/boot/cmdline.txt`
- If you have not already enabled ssh then create a empty file called `ssh` in /boot
- Add `libcomposite` to `/etc/modules`
- Add `denyinterfaces usb0` to `/etc/dhcpcd.conf`
- Install dnsmasq with `sudo apt-get install dnsmasq`

- Create `/etc/dnsmasq.d/usb` 
- Create `/etc/network/interfaces.d/usb0`
- Create `/root/usb.sh`
- Make `/root/usb.sh` executable with `chmod +x /root/usb.sh`
- Add `/root/usb.sh` to `/etc/rc.local` before `exit 0` (I really should add a systemd startup script here at some point)

With this setup the Pi4 will show up as a ethernet device with an IP address of 10.55.0.1 and will assign the device you plug it into an IP address via DHCP. This means you can just ssh to pi@10.55.0.1 to start using it.
