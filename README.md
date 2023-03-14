# iSCSI live

A bootable live image configuration to serve a disk as an iSCSI target.

The live image is built with [Debian Live](https://live-team.pages.debian.net/live-manual/html/live-manual/index.en.html), this repository holds a compatible configuration directory. The configuration is based on Debian sid amd64.

The image is configured to run headless, just customize to your needs, build, flash to a thumb drive and plug it into any computer. An IP address is obtained through DHCP.

## Features

* iSCSI tgt server configured to serve `/dev/nvme0n1`
* SSH server to allow root remote access (add your SSH key!)
* Enables wake on lan on all network devices
* Disables nvidia/nouveau kernel modules and turns off nvidia card with bbswitch (for laptops with optimus/prime)
* Disables Bluetooth kernel modules

## Requirements for building

You need a Debian Linux distribution (I used Debian 11) to build the image, as well as the live-build package:

	apt install sudo live-build

## Setup and customization

Clone this repository

	git clone https://github.com/mh0rst/iscsi-live
	cd iscsi-live

Add your SSH key here for root access: `config/includes.chroot/root/.ssh/authorized_keys`

Configure the iSCSI target here (i.e. to change the shared disk): `config/includes.chroot/etc/tgt/conf.d/target.conf`

Setup custom networking here: `config/includes.chroot/etc/network/interfaces`, then adjust the `LB_BOOTAPPEND_LIVE` config option in `config/binary`. If you forget to adjust the `LB_BOOTAPPEND_LIVE` config option, live-boot will generate it's own `/etc/network/interfaces` file and ignores the custom networking - this is the default!

## Building the image

To build the ISO file, run

	sudo lb build

Flash the resulting `live-image-amd64.hybrid.iso` to a thumb drive or burn it to a CD. The size of the image should be around 400 MBytes.

If you customized some files after building, it may be necessary to clean the project

	sudo lb clean
	sudo lb clean --binary
	git restore .build/config
