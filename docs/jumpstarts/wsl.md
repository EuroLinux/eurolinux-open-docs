# EuroLinux on WSL Jumpstart

This guide provides a quick overview of setting up EuroLinux on Windows Subsystem for Linux (WSL).


## Prerequisites:

- Windows 10 (Windows 10 version 1709 or newer for "legacy" WSL. Version 2004 or newer for WSL 2) or Windows 11
- WSL/WSL 2 installed (we strongly recommended WSL 2 for better compatibility and performance)
- Administrative privileges on your Windows system

## Enabling WSL

Follow the official Microsoft documentation to enable WSL: [https://learn.microsoft.com/en-us/windows/wsl/](https://learn.microsoft.com/en-us/windows/wsl/)

## About EuroLinux on WSL

EuroLinux company provides EuroLinux version 9 for WSL. The system is based on Red Hat
Enterprise Linux 9 and is compatible with it. We used our base container image. The official
repository is located [https://github.com/EuroLinux/wsl](https://github.com/EuroLinux/wsl) it also contains the build scripts and
latest documentation/releases.

## Import and install EuroLinux for WSL

- x86_64
  ```bash
  wget https://github.com/EuroLinux/WSL/releases/latest/download/el9-x86_64.tar -o el9-x86_64.tar
  wsl --import EuroLinux-9 "$env:USERPROFILE/EuroLinux-9" .\el9-x86_64.tar --version 2
  wsl -d EuroLinux-9
  ```

- aarch64
  ```bash
  wget https://github.com/EuroLinux/WSL/releases/latest/download/el9-aarch64.tar -o el9-aarch64.tar
  wsl --import EuroLinux-9 "$env:USERPROFILE/EuroLinux-9" .\el9-aarcch64.tar --version 2
  wsl -d EuroLinux-9
  ```


## Feedback

To provide feedback, request changes, or report bugs, please visit our official
RFC/Bug repository at
[https://github.com/EuroLinux/eurolinux-distro-bugs-and-rfc.](https://github.com/EuroLinux/eurolinux-distro-bugs-and-rfc)
Your input is highly appreciated!
