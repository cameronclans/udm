# udm
UDM scripts and utilities I use or have used

From Boost chicken (the original repo)
UDM / UDMPro Boot Script

Features

Allows you to run a shell script at S95 anytime your UDM starts / reboots
Persists through reboot and firmware updates! It is able to do this because Ubiquiti caches all debian package installs on the UDM in /mnt/data, then re-installs them on reset of unifi-os container.
Compatibility

Should work on any UDM/UDMPro after 1.6.3
Tested and confirmed on 1.6.6, 1.7.0, 1.7.2rc4, 1.7.3rc1, 1.8.0rc7, 1.8.0+
Upgrade from earlier way

As long as you didn't change the filenames, installing the deb package is all you need to do. If you want to clean up beforehand anyways....

rm /etc/init.d/udm.sh
systemctl disable udmboot
rm /etc/systemd/system/udmboot.service
build_deb.sh can be used to build the package by yourself.

dpkg-build-files contains the sources that debuild uses to build the package if you want to build it yourself / change it
by default it uses docker or podman to build the debian package
use ./build_deb.sh build to not use a container
the resulting package will be in packages/
Built on Ubuntu-20.04 on Windows 10/WSL2

Steps

Get into the unifios shell on your udm

unifi-os shell
Download udm-boot_1.0.5_all.deb and install it and go back to the UDM. The latest package will always be found at https://udm-boot.boostchicken.dev

curl -L https://udm-boot.boostchicken.dev -o udm-boot_1.0.5_all.deb
dpkg -i udm-boot_1.0.5_all.deb
exit
Copy any shell scripts you want to run to /mnt/data/on_boot.d on your UDM (not the unifi-os shell) and make sure they are executable and have the correct shebang (#!/bin/sh)

Examples:

Start a DNS Container 10-dns.sh
Start wpa_supplicant on_boot.d/10-wpa_supplicant.sh
Add a persistent ssh key for the root user on_boot.d/15-add-root-ssh-key.sh
