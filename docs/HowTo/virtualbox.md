# VirtualBox
How to set up your VirtualBox installation.

## Introduction
### Use cases
Consider the following examples:
- You use software that doesn't work on your EuroLinux 8 installation.
- You want to try out some potentially dangerous actions and don't want to
  endanger your machine.
- You develop some awesome software and want to test it on several systems for
  compatibility
- You want an easily reproducible environment that works the same way on every
  person's machine 
- You need a multi-machine laboratory, maybe with several different systems and
  don't have the resources for a physical equipment

### Why use it over other virtualisation providers?
VirtualBox is the provider with a copyleft license and a focus on
interoperability when it comes to supporting different platforms. This allows
you to cooperate with someone running a different operating system and when
exchanging documentation - once written it's applicable to anyone that can run
the software.  
New VirtualBox versions usually support older operating systems too. One can
enjoy the new features on a system, which has recently reached its End of Life,
which can be indispensable for a company that can't migrate yet.

## System requirements
A brief documentation is available at
[Upstream's](https://www.virtualbox.org/wiki/End-user_documentation).  

Your machine shall support hardware virtualisation. If it doesn't, you either
need to perform additional troubleshooting - e.g. enable virtualisation in your
machine's BIOS settings.

```
[ "$(grep -c vmx /proc/cpuinfo)" -gt 0 ] && echo "OK"
```

Make sure you're running EuroLinux 8 on x86_64 architecture rather than ARM.  

```
[ "$(lscpu | grep x86_64)" ] && echo "OK"
```

## Installation on EuroLinux 8

As of today, 6.1 is the main VirtualBox branch and this is the one we install
in this guide.  
Assuming your username is *user* and has been made an administrator during the
installation process, simply run these commands for an installation:

```
sudo dnf groupinstall "Development Tools" -y
sudo dnf config-manager --add-repo=https://download.virtualbox.org/virtualbox/rpm/el/virtualbox.repo
sudo dnf install VirtualBox-6.1 -y
sudo usermod -a -G vboxusers user
```
Log out after this procedure and log in again. VirtualBox should be ready to
use!  

### Extension Pack
If you want VirtualBox to support USB 2.0/3.0, builtin disk encryption, etc.
you will need VirtualBox Extension Pack. Make sure you have read [its
license](https://www.virtualbox.org/wiki/VirtualBox_PUEL) and understand its
implications - you're responsible for compliance.  
You may want to read [Upstream's
FAQ](https://www.virtualbox.org/wiki/Licensing_FAQ) for a quick start.

Once you're sure you'll be able to comply with the licensing terms, run these commands to install the Extension Pack:
```
export vbox_version="$(VBoxManage --version | cut -d'r' -f 1)"
wget "https://download.virtualbox.org/virtualbox/$vbox_version/Oracle_VM_VirtualBox_Extension_Pack-$vbox_version.vbox-extpack"
yes | sudo VBoxManage extpack install --replace Oracle_VM_VirtualBox_Extension_Pack-$vbox_version.vbox-extpack
```

## Troubleshooting
### I can't enable hardware virtualisation and prefer software emulation
As [Upstream
says](https://web.archive.org/web/20210830005115/https://www.virtualbox.org/wiki/Downloads),
for software mode you'll need VirtualBox branch 6.0 or older.
> Please also use version 6.0 if you need to run VMs with software
> virtualization, as this has been discontinued in 6.1.

