---
layout: post
title: "install snappy ubuntu core"
date: "2016-03-02 13:30:42 +0700"
---
Here are some steps to run snappy ubuntu core

#### Install from Windows

Download the Ubuntu Core image into your Downloads folder from this [link](http://cdimage.ubuntu.com/ubuntu-snappy/15.04/stable/latest/ubuntu-15.04-snappy-armhf-raspi2.img.xz).

Once the download is finished, you’ll have a zip file named ubuntu-15.04-snappy-armhf-rpi2.img.xz .

Extract the downloaded zip file into your Downloads folder (you might have to install  archive extractor software, like 7-zip or similar as the standard tools do not support xz) you will now have a file called ubuntu-15.04-snappy-armhf-rpi2.img .

Insert the MicroSD Card on your computer, you can use an external card reader or the SD slot if your computer has one.

Download and install Win32DiskImager.

Launch the Win32DiskImager application.

Find out where what drive your SD card is mounted to. Open a File Explorer window to check which drive your SD card is listed under.  Here is an example of a card listed under E: and the setup in Diskimager.
![Alt text](https://developer.ubuntu.com/static/devportal_uploaded/9242b67a-987c-4181-a9aa-59b1767fd2d1-cms_page_media/1014/Drives.png)
Win32DiskImager will need 2 elements:

* An Image File which is the file you want to copy on your SD Card. Navigate to your Downloads folder and select the ubuntu-15.04-snappy-armhf-rpi2.img  image you have just extracted.
* A Device which is the location of your SD card. Select the Drive in which your SD card is mounted.

![Alt text](https://developer.ubuntu.com/static/devportal_uploaded/b295bfa0-9546-4997-9a7f-8ee4d5f8b800-cms_page_media/1014/Diskimager_setup.png)
To be safe, unplug every External USB Drive you may have connected to your PC.

When ready click on Write and wait for the process to complete.

Exit from Win32DiskImager and eject the SD card from the File Explorer window.

Eject the SD card physically from your PC and Insert the SD card in your Raspberry Pi.

#### Log in to Ubuntu Core

Power on your Raspberry Pi 2 and wait 1-2 minutes for the OS to complete its first boot. You can then access your snappy Ubuntu Core system either directly with a keyboard and display connected, or with an SSH client (such as PuTTY):

`$ ssh ubuntu@webdm.local`

The default username and password are ubuntu.

#### Install from Ubuntu
Requirements

    Ubuntu desktop (or any OS with xzcat, dd, and sync or equivalents available)

##### Install commands

Start by downloading the [Ubuntu Core image for Raspberry Pi 2](http://cdimage.ubuntu.com/ubuntu-snappy/15.04/stable/latest/ubuntu-15.04-snappy-armhf-raspi2.img.xz) in your ~/Downloads folder.

Insert your SD card, unmount it and run:

    Note: replace /dev/sdX with the device name of your SD card (e.g. /dev/mmcblk0, /dev/sdg1 ...)

```sh
    xzcat ~/Downloads/ubuntu-15.04-snappy-armhf-raspi2.img.xz | sudo dd of=/dev/sdX bs=32M
    sync
```

Eject the SD card physically from your PC and Insert the SD card in your Raspberry Pi.
Notes

* If your SD card is mounted when you insert it into your computer (you will know it if the file manager automatically opens a window showing the card's contents), you must manually unmount it before writing the snappy image to it. Either eject your SD card from the file manager, or from the command line: sudo umount /media/$USER/*
* You must specify the path to the disk device representing your SD card in the dd command above. Common device paths for the SD card disk device are either of the form /dev/sdX (such as /dev/sdb, not /dev/sdb1!) or /dev/mmcblk0 (not /dev/mmcblk0p1!)
* Ensure there is no data you care about on the SD card before running the dd command above.

#### Log in to Ubuntu Core

Power on your Raspberry Pi 2 and wait 1-2 minutes for the OS to complete its first boot. You can then access your snappy Ubuntu Core system either directly with a keyboard and display connected, or through SSH:

`$ ssh ubuntu@webdm.local`

The default username and password are ubuntu.

It's time to to explore your new system.
