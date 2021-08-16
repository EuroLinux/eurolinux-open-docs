# How to create ISO file with EuroLinux repositories

Creating ISO that contains RPM repositories is a straightforward process. First
you have to mirror repositories locally - mirroring is described in [Mirror
EuroLinux Locally How To](mirror-eurolinux-locally.md).

To create an ISO file, you need `mkiso` command that is part of `genisoimage`
package. You can install it with the following command:


```bash
sudo yum install -y genisoimage
```

## Creating ISO with repositories

In the example below, we create ISO from the repositories saved in `/repos`
directory the output is saved to `/var/eurolinux-repos.iso`.
```
mkisofs -R -J -o /var/eurolinux-repos.iso /repos/
```

This file could be:

- locally copied to the machine that will be using it
- added to ISOs pool of Virtual Machine manager/orchestrator or cloud computing
  platform of your choice

## Mounting ISO from local file

Mounting local ISO file is as simple as:

```bash
sudo mount -o loop /PATH/TO/ISO /MOUNT/PATH
```

example:

```bash
mount -o loop /var/eurolinux-repos.iso /mnt
```

## Mounting ISO from virtual cd-rom device


Mounting CD-ROM device is as simple as:

```bash
sudo mount /dev/DEVICE /MOUNT/PATH
```

The following example has ISO mounted as CD-ROM device `/dev/sr0`:

```bash
sudo mount /dev/sr0 /mnt
```

## Using locally mounted ISO with RPM repositories

To use repositories that ISO file provides create proper `.repo` file that
resides inside `/etc/yum.repos.d/` directory. Example
`/etc/yum.repos.d/local-iso.repo` file for EuroLinux 7 and iso attached to
`/mnt` directory.

```ini
[base]
name = EuroLinux 7 x86_64 Base
baseurl=file:///mnt/eurolinux-os-7/
enabled=1
# Disabled gpgcheck, enable if el-release is already installed on your system
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux7

[updates]
name = EuroLinux 7 x86_64 Updates
baseurl=file:///mnt/eurolinux-updates-7/
enabled=1
# Disabled gpgcheck, enable if el-release is already installed on your system
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux7
```
