# EuroLinux for Raspberry Pi Jump Start

## About images

EuroLinux Raspberry are made for Raspberry Pi 4 model B. The system used is
EuroLinux 9. The minimal images could work with older version Raspberry Pi 3 as Raspberry Pi 3 does not meet minimum hardware requirements for the Enterprise
Linux 9.

The basic credentials are the following:

- `user`: root
- `password`: raspberry

!!! Info "SSH Root login disabled"
    SSH root login is disabled by default on EuroLinux 9.

!!! info "Raspberry only"
    The Raspberry Pi images also won't work with other ARM-64 computers as RPI
    images are specially tailored for this particular hardware.

## Downloading and checking the images

Firstly choose the image from the
[https://fbi.cdn.euro-linux.com/images](https://fbi.cdn.euro-linux.com/images).
The Raspberry Pi images have the `rpi-TYPE`(where TYPE can be `minimal` or
`gnome`) in their names. You can download the image with `wget`, `curl` or with
your browser.


It's advised to check the image integrity by comparing checksums. The SHA256
checksums can be find under the
https://fbi.cdn.euro-linux.com/images/sha256sum.txt


Sample download and checksum comparition
```
wget https://fbi.cdn.euro-linux.com/images/EuroLinux-9-rpi-minimal-2023-01-02-sda.raw.xz
sha256sum EuroLinux-9-rpi-minimal-2023-01-02-sda.raw.xz
curl -s https://fbi.cdn.euro-linux.com/images/sha256sum.txt | grep EuroLinux-9-rpi-minimal-2023-01-02-sda.raw.xz
```
[![asciicast](https://asciinema.org/a/549328.svg)](https://asciinema.org/a/549328)

## Flasing the microsd card


### BalenaEtcher

Belena Etcher is one of the most popular and easy to user program that allows
to flash the SD cards for Raspberry Pi. It can also create other bootable media
like the USB sticks and more. The process below


### dd

This setup requires 

## Wifi setup

Wifi adapter works out-of-box. It's trivial to configure it from Gnome. To
configure the from the console you can run the following:

Firstly find the SSID (WIFI name) that you want to use:
```
nmcli d wifi list
```
Then you can provide password on the command line (note password will be saved
in command history)

```
nmcli d wifi connect WIFI_NAME password does-this-unit-have-a-soul?
```
or if You want to prompted for the password
```
nmcli d wifi WIFI_NAME SHEPARD --ask
```

[![asciicast](https://asciinema.org/a/549307.svg)](https://asciinema.org/a/549307)

### Disable the powersave mode on the Raspberry Pi WIFI card

The Raspberry Pi WIFI card by default enters powersave mode when there is no
much going on. To disable this You can use the following command

```bash
iw wlan0 set power_save off
```

The problem with this solution that this state won't survive system reboot. To
fix this during startup you can can add Network Manager dispatcher
script that will disable power_save. Put the following script:

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


## Enabling the I2c
```
TODO - console I2c
```

## Enabling SPI
```
TODO - console SPI
```
## Overclocking
!!! warning "BE AWARE"
```
TODO - overclocking
```
