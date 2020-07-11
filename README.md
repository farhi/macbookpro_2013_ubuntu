# macbookpro_2013_ubuntu
A short note on installing a Debian/Ubuntu system onto a MacBookPro 11.1 from late-2013 (retina)

The late-2013 MacBookPro (retina display) is an old Apple laptop. It's main advantage is that there is no T2 chip (as oposed to recent models) to block the installation of other systems than MacOS X.

The hardware is as follows:
- CPU: Intel Corporation Haswell-ULT Intel(R) Core(TM) 2 cores/4 threads
- GPU: Intel Iris Graphics (HD4000)
- Wifi: Broadcom Corporation BCM4360 802.11ac Wireless Network Adapter
- Screen: 2560x1600

There is a known hardware failure on these machines. The SD-card reader becomes faulty, and slows down the boot process (with USB error messages). A side effect can be that after boot, you may have to wait for e.g. a minute for the keyboard/mouse to become active.

Ubuntu/Debian installation
==========================

The installation over MacOS X is rather simple. Start to upgrade the MacOS X system (which often srews-up the GRUB boot loader).

Reduce the Mac OSX partition size, so that there is room for the new system. You may have to restart under the _Recovery_ mode. Use `Command-R` key for that. Open the _Disk Utility_ 

Then create with [UNetBootIn](https://unetbootin.github.io/) a USB bootable key with e.g. Debian 10 or Ubuntu 20.04. 
UnetBooIn has the advantage that the boot is effective from the Mac EFI. Other USB creators may fail.

Boot the machine and hold the _Alt_ key. The EFI boot selector shows up. Select the yellow `EFI boot` item (usually on the right side, after Mac OSX).
Then install the system in the available space with the _Try Ubuntu_ item. GRUB will be set-up.
After installation, do not reboot.

Boot loader
-----------

- `sudo nano /etc/default/grub`
- change line `GRUB_CMDLINE_LINUX="libata.force=noncq"`
- `sudo nano /etc/grub.d/40_custom`
- add at the end
```
menuentry "MacOS" {
  exit
}
```
- `sudo update-grub`

Webcam
------

The webcam may not be functional. See [Apple Facetime PCIe Webcam.](https://github.com/patjak/bcwc_pcie).

Type the following code
```
sudo apt-get install git
git clone https://github.com/patjak/bcwc_pcie.git
cd bcwc_pcie/firmware
make
sudo make install
cd ..
make
sudo make install
sudo depmod
sudo modprobe -r bdc_pci
sudo modprobe facetimehd
```

Then edit `sudo gedit /etc/modules` and add `facetimehd` at the end.


Hardware support
================

| MacBookPro11,1 | Status | Comment |
| ---------|--------|---------|
| wifi     | :+1:   | Broadcom BCM4360 |
| audio    | :+1:   | Intel Corporation Haswell-ULT HD Audio Controller (i915) |
| display  | :+1:   | Intel Corporation Haswell-ULT HD Audio Controller (i915) |
| media/function keys | :+1: | use `Fn` lower left key for `Fxx` |
| webcam   | :warning: | see [Apple Facetime PCIe Webcam.](https://github.com/patjak/bcwc_pcie) |
| battery  | :+1:   | lasts about 9h. It has 63 Wh capacity, and burns 7W when idle. |

The laptop develops 4.3 Gflops with its 4 threads (MPI matrix test).
