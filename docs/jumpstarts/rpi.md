# EuroLinux for Raspberry Pi Jump Start

## About images

EuroLinux Raspberry images are made for Raspberry Pi 4 model B. The system is
EuroLinux 9. The minimal images could work with older Raspberry Pi 3. But we
won't support it even with 'best effort' as Raspberry Pi 3 does not meet
the minimum hardware requirements for Enterprise Linux 9 or Enterprise Linux 8.

The basic credentials are the following:

- `user`: root
- `password`: raspberry

!!! Info "SSH Root login disabled"
    SSH root login is disabled by default on EuroLinux 9.

!!! info "Raspberry only"
    The Raspberry Pi images also won't work with other ARM-64 computers as RPI
    images are specially tailored for this particular hardware.

## Downloading and checking the images

Firstly choose the image from
[https://fbi.cdn.euro-linux.com/images](https://fbi.cdn.euro-linux.com/images).
The Raspberry Pi images have the `rpi-TYPE`(where TYPE can be `minimal` or
`gnome`) in their names. You can download the image with `wget`, `curl` or with
your browser.


It's advised to check the image integrity by comparing checksums. The SHA256
checksums can be found at
https://fbi.cdn.euro-linux.com/images/sha256sum.txt


Example download and checksum comparison:

```bash
wget https://fbi.cdn.euro-linux.com/images/EuroLinux-9-rpi-minimal-2023-01-02-sda.raw.xz
sha256sum EuroLinux-9-rpi-minimal-2023-01-02-sda.raw.xz
curl -s https://fbi.cdn.euro-linux.com/images/sha256sum.txt | grep EuroLinux-9-rpi-minimal-2023-01-02-sda.raw.xz
```

[![asciicast](https://asciinema.org/a/549328.svg)](https://asciinema.org/a/549328)

## Flashing the MicroSD card
With the image downloaded now, it's time to flash your MicroSD card. There are
multiple options, you can use:

- Raspberry Pi Imager, which requires manual compilation for the most platforms
- Balena Etcher comes as AppImage so works with nearly every Linux distribution
- `dd` program, which is the most CLI-friendly way, but is also recommended for
  experienced users

We recommend Balena Etcher as dd might destroy the system partition or other
important data if used without proper experience.

### Balena Etcher

Balena Etcher is one of the most popular and easy-to-use programs that allow
flashing the SD cards for Raspberry Pi. It can also create other bootable media
like USB sticks and more. Firstly download the AppImage from the official
Balena Etcher website -
[https://www.balena.io/etcher/](https://www.balena.io/etcher/) (Download for
Linux x64).

Most file managers will run AppImage when chosen and clicked. To run it from
the command line firstly change the permissions then run.
```
chmod 755 balenaEtcher-1.13.1-x64.AppImage
./balenaEtcher-1.13.1-x64.AppImage
```

The process itself is straightforward. There is plenty of documentation/videos
about the Balena Etcher, so we trust that in case of any troubles you will be
able to find a solution on your own.

### dd

`dd` is a program that is older than Linux Kernel itself :). It is one of these
little tool that makes Linux/Unix powerful. It can be used to flash the memory
card by writing output to the memory card device.

Firstly insert the memory card into the slot. Then check with the dmesg device
file that is corresponding.

The dmesg will inform about partition:
```
[ TIME] scsi 0:0:0:0: Direct-Access     Generic  Mass-Storage     1.11 PQ: 0 ANSI: 2
[ TIME] scsi 0:0:0:0: Attached scsi generic sg0 type 0
[ TIME] sd 0:0:0:0: [sdX] 250347520 512-byte logical blocks: (128 GB/119 GiB)
[ TIME] sd 0:0:0:0: [sdX] Write Protect is off
[ TIME] sd 0:0:0:0: [sdX] Mode Sense: 03 00 00 00
[ TIME] sd 0:0:0:0: [sdX] No Caching mode page found
[ TIME] sd 0:0:0:0: [sdX] Assuming drive cache: write through
```

Where sdX is your SD card. To write you first need to decompress the image with
the `xzcat` command and then pipe output to the dd (writing to the device
requires root privileges, that's why there is sudo).
```
xzcat /path/to/image/image.raw.xz | sudo dd status=progress oflag=sync bs=4k of=/dev/sdX
```

For example:
```
xzcat ~/Downloads/EuroLinux-9-rpi-minimal-2023-01-02-sda.raw.xz | sudo dd status=progress oflag=sync bs=4k of=/dev/sda
```

## Booting from USB.

EuroLinux 9.1 can be natively booted from USB on Raspberry Pi 4. You should
flash your USB stick/disk in the same manner that you flash a micro SD card.

!!! info "Early-produced RPI 4 might require firmware update."
    Early-produced Raspberry Pi 4 might require a firmware update before booting
    the system from the USB.

## Wifi setup

Wifi adapter works out-of-box. It's trivial to configure it from a desktop
(Gnome). To configure the WIFI from the console you can use the following
commands.


Firstly find the SSID (WIFI name) that you want to use:

```bash
nmcli d wifi list
```

Then you can provide a password on the command line (note password will be
saved in bash command history)

```bash
nmcli d wifi connect WIFI_NAME password PASSWORD
```

or if You want to be prompted for the password

```bash
nmcli d wifi connect WIFI_NAME --ask
```

[![asciicast](https://asciinema.org/a/549307.svg)](https://asciinema.org/a/549307)

Note that your RPI will automatically connect to the WIFI after reboot.

### Disable the powersave mode on the Raspberry Pi WIFI card

The Raspberry Pi WIFI card by default enters powersave mode when there is not
much going on. Some users reported a problem with broken SSH sessions and other
closed connections due to this feature. To disable power save mode use the
following command

```bash
iw wlan0 set power_save off
```

The problem with this solution is that this state won't survive system reboot.
To fix that issue during startup you can add a network manager dispatcher
script that will disable power_save on boot. Put the following script:

```bash
#!/usr/bin/env bash

interface=$1
event=$2

if [[ $interface != "wlan0" ]] || [[ $event != "up" ]]
then
  return 0
fi
iw wlan0 set power_save off
```

into the `/etc/NetworkManager/dispatcher.d/iw-wlan0-disable-powersave.sh`, then
add the execution permission

```bash
chmod +x /etc/NetworkManager/dispatcher.d/iw-wlan0-disable-powersave.sh
```

## Enabling the I2C (I²C - Inter-Integrated Circuit)
To enable i2c you have to add the `i2c_arm` with flag `on` as `dtparam` to the
`/boot/config.txt`

```ini
dtparam=i2c_arm=on
```

The following script can be used:

```bash
echo 'dtparam=i2c_arm=on'  | sudo tee -a /boot/config.txt
```

We also recommend installing i2c-tools.

```bash
sudo dnf install -y i2c-tools
```

After this changes, you have to reboot the system to start i2c.

## Enabling SPI (Serial Peripheral Interface).
To enable SPI you have to add `spi` with flag `on` as `dtparam` to the
`/boot/config.txt`

```ini
dtparam=spi=on
```
The following script can be used:
```bash
echo 'dtparam=spi=on'  | sudo tee -a /boot/config.txt
```
After this changes you have to reboot the system to start SPI.


## Feedback

If You want to leave feedback/request for change/bug report on EuroLinux
Raspberry Pi images please use the [https://github.com/EuroLinux/raspberry-pi-build](https://github.com/EuroLinux/raspberry-pi-build) repository.

If believe that something important from the documentation is
missing don't hesitate to create issue in this documentation repository.
