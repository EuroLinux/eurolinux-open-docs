# EuroLinux Linux distribution capabilities and limits

This document describes the technology capabilities and limits for EuroLinux 6,
7, 8. Some are theoretical, as they are connected with source code/projects
used in the system. Minimal limits represent limits for the systems for which
support is generally available by the EuroLinux company. Our dedicated
solutions like:

- system rebuilds with Gaia build stack
- EuroLinux container images
- EuroLinux cloud images
- EuroLinux for edge computing
- EuroLinux ARM 64 for IoT
- containers running on the EuroLinux container platform or any other
  Kubernetes-based platform

might run and be supported on the less resources than described in this document.

The theoretical limit (connected with a version of the software used) is marked
as `(LIMIT)`, when limit supported and tested by upstream is provided as
default for EuroLinux. **TBA** means - **To Be Announced**.

## Minimum logical CPU

All systems, physical or virtual, require at minimum 1 logical (physical or
virtual) CPU core.

## Maximum logical CPU



| Architecture | EuroLinux 6  | EuroLinux 7 | EuroLinux 8 | EuroLinux 9 |
|---|---|---|---|---|
| x86_64  | 448 (4096)  |  768 (5120) | 768 (8192)  | TBA |
| ARM64  | X  | X  | 256  | TBA  |
<!-- | POWER  | X | X | TBA  | TBA  | -->


## Minimum memory

These requirements are only for systems supported as VMs or Physical hosts. In
most cases, it's possible to run a system without complications on less memory.
The minimum requirements have been provided below because it might not be
possible to support systems with less memory.


| Architecture | EuroLinux 6  | EuroLinux 7 | EuroLinux 8 | EuroLinux 9 |
|---|---|---|---|---|
| x86_64  | Minimum 1 GiB, 1 GiB per logical core is recommended |  Minimum 1 GiB, 1 GiB per logical core is recommended  |  Minimum 1 GiB, 1.5 GiB per logical core is recommended  | minimum 2 GiB, 1.5 GiB per logical core is recommended|
| ARM64  | X  | X  | 2GiB | 2GiB |
<!-- | POWER  | X | X | TBA  | TBA  | -->

## Maximum memory

| Architecture | EuroLinux 6  | EuroLinux 7 | EuroLinux 8 | EuroLinux 9 |
|---|---|---|---|---|
| x86_64  | 12TB (64TB) | 12TB (64TB) | 24TB (64TB) | TBA |
| ARM64  | X  | X  | 1.5TB (256TB) | TBA |
<!-- | POWER  | X | X | TBA  | TBA  | -->

## Minimum required disk space

| EuroLinux 6  | EuroLinux 7 | EuroLinux 8 | EuroLinux 9 |
|---|---|---|---|
| 1Gib Minimum, 5GiB recommended  |  5 GiB Minimum, 20 GiB recommended | 10 GiB Minimum, 20 GiB recommended  |  10 GiB Minimum, 20 GiB recommended |

## File systems and storage limits

All filesystems in this document support ACL (Access Control List).

### Ext3

Ext3 is mostly a legacy filesystem. Please use Ext4 or XFS.

| Feature | EuroLinux 6  | EuroLinux 7 | EuroLinux 8 | EuroLinux 9 |
|---|---|---|---|---|
| Maximum File Size  | 2TiB | 2TiB | 2TiB | 2TiB |
| Maximum Filesystem Size | 16TiB | 16TiB | 16TiB | 16TiB |
| Maximum Subdirectories or files in directory | 32000 | 32000  |  32000  |  32000  |
| Maximum symlink depth | 8 | 8 | 8 | 8 |


### Ext4

Ext4 is the default filesystem for EuroLinux 6.

| Feature | EuroLinux 6  | EuroLinux 7 | EuroLinux 8 | EuroLinux 9 |
|---|---|---|---|---|
| Maximum File Size  | 16TiB | 16TiB | 16TiB | 16TiB |
| Maximum Filesystem Size | 1EiB | 1EiB | 1EiB | 1EiB |
| Maximum Subdirectories or files in directory | 65000/unlimited with `dir_nlink` option | 65000/unlimited with `dir_nlink` option |  65000/unlimited with `dir_nlink` option  | 65000/unlimited with `dir_nlink` option  |
| Maximum symlink depth | 8 | 8 | 8 | 8 |

### XFS

XFS is the default filesystem for in EuroLinux 7, 8 and 9.

| Feature | EuroLinux 6  | EuroLinux 7 | EuroLinux 8 | EuroLinux 9 |
|---|---|---|---|---|
| Maximum File Size  | 8EiB | 8EiB | 8EiB | 8EiB |
| Maximum Filesystem Size | 16EiB | 16EiB | 1PiB | 1PiB |
| Maximum Subdirectories or files in directory | unlimited | unlimited | unlimited | unlimited |
| Maximum symlink depth | 8 | 8 | 8 | 8 |


### GFS2

Global Filesystem 2 is part of EuroLinux Resilient Storage and High
Availability add-ons that are freely available with subscription or free
available in the Open Core model.

| Feature | EuroLinux 6  | EuroLinux 7 | EuroLinux 8 | EuroLinux 9 |
|---|---|---|---|---|
| Maximum File Size  | 8EiB | 8EiB | 8EiB | 8EiB |
| Maximum Filesystem Size | 8EiB | 8EiB | 8EiB | 8EiB |
| Maximum Subdirectories or files in directory | unlimited | unlimited | unlimited | unlimited |
| Maximum symlink depth | unlimited | unlimited | unlimited | unlimited |


### Kernel and the most important components versions

| Feature | EuroLinux 6  | EuroLinux 7 | EuroLinux 8 | EuroLinux 9 |
|---|---|---|---|---|
| Base Linux Kernel version | 2.6.34 | 3.10 | 4.18 | 5.14 |
| Package Management | RPM/Yum | RPM/Yum | RPM/Dnf, Flatpak| RPM/Dnf, Flatpak|
| System Init | Upstart | systemd | systemd | systemd |
| Base GNU C Library (glibc) Version | 2.12 | 2.17 | 2.28 | 2.34 |
| Base (First) GCC version | 4.4.7 | 4.8.5 | 8.2.1 (updated in newer versions) | 11.2.1 (might be updated in the future)|
| Base (First) LLVM version | X | X | 7.0.1 (updated in newer versions)| 13.0.0 (might be updated in the future)|
| Default Desktop | Gnome 2 | Gnome 3 | Gnome 3 |  Gnome 40 |
| Office Suite | LibreOffice | LibreOffice | LibreOffice | LibreOffice |
| Default Web Browser | Firefox | Firefox | Firefox | Firefox |
| Default Mail Client | Thunderbird | Evolution or Thunderbird | Evolution or Thunderbird | Evolution or Thunderbird |
