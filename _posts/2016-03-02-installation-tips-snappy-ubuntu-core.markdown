---
layout: post
title: "installation tips snappy ubuntu core"
date: "2016-03-02 10:15:34 +0700"
---
Target installation overview

You have two options to install snappy Ubuntu Core on your target:

    Download a prebuilt snappy image for your platform
    Alternatively, create a customised image using the ubuntu-device-flash tool.

This section assumes you have a prebuilt image for your target device. How to use ubuntu-device-flash to produce custom image configurations is not covered in this article.

Snappy images released by the Ubuntu Core team and produced by ubuntu-device-flash are in a format that can be copied to media using the traditional dd command available in all Ubuntu hosts.
Systems with removable boot media

In case you are setting up a target whose boot media is removable, the easiest way to install is to plug the removable media into your host system and use the dd command to copy the snappy image onto that disk –for instance a USB disk or an SD card.

In order to do that, you will first have to download the image onto your development host. You will find readily-made images for common platforms along with installation instructions on the Getting Started page.

Once the appropriate image has been downloaded, you can then plug in your disk, identify the block device node the media is mapped to on your host (e.g. /dev/sdbX) and run the following commands on the terminal:

``` shell
    $ # Write the image on block device - DELETES ALL DATA ON THAT MEDIA
    $ dd if=your-snappy-image.img of=/dev/sdbX bs=32M
    $ # Ensure the write has properly finished by running sync command
    $ sync
```
After the image installation has finished, you then can plug the media in your target machine and boot from it.

In some systems you might need to modify the configuration to use that media as the primary boot disk –for instance in the BIOS, EFI. These steps are not covered in this manual. Please refer to the documentation of your target device.
Systems with built-in boot media

For systems with a built in boot media the current way is to put a general purpose live OS (like the ubuntu server installer or the ubuntu desktop live CD) on a USB key, boot into that environment and then use the same approach described in 'Systems with removable boot media' section above.
Discovering and configuring your device on the network

For the first boot of a Snappy system it needs to be connected to the wired network and it needs to be able to find a DHCP server from which to set up its routing and interface information.

Snappy devices ship with WebDM installed, this allows for easy discovery as these advertise an Avahi service on the webdm.local hostname. You can SSH into the device according to the below instructions if the SSH server is enabled.

To configure a wireless network Snappy ships wpa_supplicant by default. This functionality can be expanded with any high level network stack that can be shipped as part of your snap or set of snaps.

To configure WPA Supplicant you must add the following to /etc/network/interfaces.d/wlan0

``` shell
    iface wlan0 inet dhcp
        wpa-conf /home/ubuntu/wireless-setup.conf
```
You must substitute wlan0 with the name of your WLAN interface to configure.

The /home/ubuntu/wireless-setup.conf file must include the following syntax so that your wireless network can be found and associated with

``` shell
    network={
      ssid="myssid"
      psk=”mypassword”
    }
```
Upon rebooting, your wireless network interface should always be configured and should obtain an IP address from the local DHCP server. You may also issue the command ifup command on the relevant interface.
Setting up a SSH connection

Using Secure SHell (SSH) is the recommended way to interface with a snappy target device. The default images that Ubuntu project ships has ssh enabled. Production builds usually don’t, as for products you often don't want users to ssh into the device. To enable ssh on such a system you can either manually start sshd or if you want it to be persistent across reboots you will have to remove '/etc/ssh/sshd_not_to_be_run' from the target filesystem.

The easiest way to do that is to modify the dd'able image by mounting it as loop device on your development host. For that first register the .img file as a virtual disk using kpartx:

``` shell
    $ sudo kpartx -av yourimage.img
    add map loop0p1 (252:3): 0 131072 linear /dev/loop0 8192
    add map loop0p2 (252:4): 0 2097152 linear /dev/loop0 139264
    add map loop0p3 (252:5): 0 2097152 linear /dev/loop0 2236416
    add map loop0p4 (252:6): 0 3282944 linear /dev/loop0 4333568
```
This has created the mapped loop devices and the right labelled entries for the boot, system-a and system-b partitions will have been created in /dev/disk/by-label/ directory. You need to mount both partitions and remove the /etc/ssh/sshd_not_to_be_run file from them:

``` shell
    # mount and enable ssh on system-a partition
    $ sudo mount /dev/disk/by-label/system-a /mnt
    $ sudo rm -f /mnt/etc/ssh/sshd_not_to_be_run
    $ sudo umount /mnt

    # mount and enable ssh on system-b partition
    $ sudo mount /dev/disk/by-label/system-a /mnt
    $ sudo rm -f /mnt/etc/ssh/sshd_not_to_be_run
    $ sudo umount /mnt
```
In the end remember to unregister your image using kpartx:

``` shell
    $ sudo kpartx -dv yourimage.img
    $ del devmap : loop0p4
    $ del devmap : loop0p3
    $ del devmap : loop0p2
    $ del devmap : loop0p1
    $ loop deleted : /dev/loop0
```
Now you can follow the install instructions from further above to flash your ssh enabled image and log in using ssh using the default ubuntu/ubuntu user.

If the users on the target system do not have an SSH key in place just put your public key into the traditional $HOME/.ssh/authorized_keys location on your Snappy host and things will behave as expected.

``` shell
    $ scp ~/.ssh/id_rsa.pub ubuntu@mysnappy.local:.ssh/authorized_keys
```

Afterwards you can login without password.
