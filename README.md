# RTL8761BU
Linux driver for RTL8761BU with better instructions
-------------------------------------------------------------------------------------------
The document describes how to install Realtek Bluetooth USB driver to Linux system.
The supported kernel version is 2.6.32 - 5.7+
1. Blacklisting conflicting kernel modules
Add the following lines to a new .conf file in /etc/modprobe.d(example: /etc/modprobe.d/bt-blacklist.conf) 

blacklist btrtl
blacklist btusb
blacklist btintel
blacklist btbcm

2. Install
   a. Build and install Realtek Bluetooth USB driver.
      Change to the driver package and execute the below command
      $ sudo make install

   If you encounter an error like "depmod: ERROR: Bad version passed /lib/modules/...",
   Please change the lines "depmod -a $(MDL_DIR)" to "depmod -a $(shell uname -r)"
   Please make sure the "make" and "kernel-devel" packages are installed to avoid further issues
   On Fedora 32, you will also need "bluez-hid2hci"
   If you are using RPMFusion, you can install the "pulseaudio-module-bluetooth-freeworld" package for more codec support, although because it's conflicting with "pulseaudio-module-bluetooth", you will need to do it the following way: (to avoid removing you entire DE...)
   In your terminal, type "sudo dnf shell"
   In the shell, type "install pulseaudio-module-bluetooth-freeworld" and hit enter
   Then type "remove pulseaudio-module-bluetooth" and hit enter
   Then type "run" and hit enter
   This will both remove the conflicting package and install the non-free one from RPMFusion, which will
avoid you accidentally removing your DE
   Then type "quit" to exit the dnf shell
3. Reboot
-------------------------------------------------------------------------------------------
After following the instructions from above, the dongle should work perfectly, although feel free to read the extra documentation(which shouldn't be needed)

4. Uninstalling the driver
   a. Unplug Realtek rtl8761BU usb dongle.
   b. Change to the driver package location and run
      $ sudo make uninstall

5. Install BlueZ (optional)

New versions of BlueZ can be downloaded from http://www.bluez.org/

Take Ubuntu for example.
1) Setting up Linux build environment
   Install dbus
   $ sudo apt-get install libdbus-1-dev

   Install glib
   $ sudo apt-get install libglib2.0-dev

2) Decompress and install BlueZ
   $ sudo tar -Jxvf bluez-5.xx.tar.xz
   $ sudo cd bluez-5.xx
   $ sudo ./configure --prefix=/usr --mandir=/usr/share/man --sysconfdir=/etc --localstatedir=/var --libexecdir=/lib
   $ sudo make
   $ sudo make install


4. Configure Remote Wakeup Function

1) Check your vendor and product id of Bluetooth device with lsusb
   $ lsusb
   Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
   Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
   Bus 001 Device 002: ID 0bda:1724 Realtek Semiconductor Corp.
   Bus 002 Device 002: ID 0e0f:0003 VMware, Inc. Virtual Mouse
   Bus 002 Device 003: ID 0e0f:0002 VMware, Inc. Virtual USB Hub
   Bus 002 Device 004: ID 0e0f:0008 VMware, Inc.

2) Create a new rule in /etc/udev/rules.d/
   For example "90-bluetoothwakeup.rules"
   By the way, the name is not important,

   $ sudo vim /etc/udev/rules.d/90-bluetoothwakeup.rules
   Add the following content to the file:
   SUBSYSTEM=="usb", ATTRS{idVendor}=="0bda", ATTRS{idProduct}=="1724" RUN+="/bin/sh -c 'echo enabled > /sys$env{DEVPATH}/../power/wakeup'"
   Then save and close the file.

   In this case, the idVendor is "0bda" and idProduct is "1724".
   echo "enabled" or "disabled" to enable/disable Bluetooth remote wakeup ability.

3) Re-insert the usb dongle. You can wake up your suspend system by bluetooth device.


5. Check firmware
   After installing the package, the firmware file would be copied to /lib/firmware/.
   The /lib/firmware is the default directory including firmware.
   But for some embedded Linux, the directory may be different.
