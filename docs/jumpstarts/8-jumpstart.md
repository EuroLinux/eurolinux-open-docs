# EuroLinux 8 Jump Start

This document contains all necessary information to setup your first EuroLinux
8 installation. 

## System Requirements

### x86_64 (64 bit AMD/INTEL architecture)

Minimal and recommended requirements are following:

| Resource | Absolute minimal requirements for cloud deployment |  Minimal |  Recommended |
|----------|--------|------|---|
| Logical CPU | 1 | 1 | 1 |
| RAM  | 768MB or 512MB with swap space|  1 GB  | 1.5 GB per logical CPU |
| Storage | 5 GB (excluding swap)| 10GB | 20GB |


Logical CPU means Physical CPU (including Hyper-Threading) or vCPU (virtual
CPU). 

!!! warning
    It might be impossible to install EuroLinux on system that does not meet
    recommended requirements.


#### About absolute minimum for cloud deployment

During our internal testing we were able to boot and use very basic and already
installed services on Virtual Machine with only 256 MB of RAM however without
additional memory or at least swap space available things like updating system
will result in actions of Kernel Out of Memory (OOM) Killer.


You can try it yourself with [EuroLinux Vagrant
boxes](https://app.vagrantup.com/eurolinux-vagrant) and following Vagrantfile:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "eurolinux-vagrant/eurolinux-8"

  # Explicitly disable vbguest because we are using rsync
  if Vagrant.has_plugin?("vagrant-vbguest")
      config.vbguest.auto_update = false
  end

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "256"
    vb.cpus = 1
  end
  config.vm.provider "libvirt" do |vb|
    vb.memory = "256"
    vb.cpus = 1
  end
end
```

It's also possible to install EuroLinux on less than 5 GB of space, but it's
also require extra attention.

All solution described above are not officialy supported by EuroLinux.

### Secure boot

At the moment EuroLinxu does not support secure boot. The full secureboot
support is planned at the end of Q3/beggining of the Q4.

## How to install EuroLinux 8 from ISO

### Where you can obtain ISO?

We distribute EuroLinux in responsible open core model. Because of that you
might download EuroLinux from different sources. The two primary sources are:

- [https://fbi.cdn.euro-linux.com/isos/](https://fbi.cdn.euro-linux.com/isos/)
  open for everyone
- [https://customerportal.euro-linux.com](https://customerportal.euro-linux.com)
  for EuroLinux customers

The only difference is that Customer Portal keep older versions, when CDN that
is use for mirroring, keep only 2 latest ISO for each version in order to save
space (single EuroLinux AppStream isos might take up to 10 GB).


Lastely if you are running huge deployment - you might consider setuping your
own mirror and download ISOs from intranet.

### How to check ISO integrity

During download a lot of things can happen - from network or sending host
failure to single bit error. In order to check if ISO is undamaged there are
two mechanism in place. First one allows you to check integrity without running
ISO. For each ISO available for EuroLinux distro there is file that has
checksums saved. Filename says which algorith was used to calculate
cryptograhpic digest. For example you can browse
https://fbi.cdn.euro-linux.com/isos/ and use `sha1sums.txt` file.


When ISO download is completed you can invoke `sha1sum` command on the ISO file
and check if it match.

```
[Alex@SpaceShipEL8 Downloads]$ sha1sum EL-8.3-x86_64-20210624-appstream.iso 
6a8abaaebe288553ec8568bd9de3f5fda5f1ddb5  EL-8.3-x86_64-20210624-appstream.iso
```


Second mechanism for checking ISO integrity is builtin installer itself. When
you start installation use the following:

![TODO obrazek]()

After booting up the ISO checking process will start.


![TODO obrazek]()

### Installation with GUI

Because even minimal installation image uses GUI the whole process is simple
and straightforward. EuroLinux 8, as well as previous versions, uses Anaconda
installer that clearly informs user what needs to be done in order to install
system.


#### Anaconda installer **LOCALIZATION** section:

- **Keyboard** - this section allows you to setup the keyboard layouts. Including
  default keyboard layout, additionaly layout and key combination to switch
  between them.
- **Language support** - additonal languages packages that should be installed.
- **Time & Date** - you can configure date, time zone, enable NTP and NTP servers.

#### Anaconda installer **SOFTWARE** section:

##### Configuring source of the installatin

Here you might configure the source of your installation. By the default system
installs from ISO image. **This is one of the officialy supported way to install
EuroLinux**. 


If you enable NIC (Network Interface Card) in the **Network**
installer section, you might add additonal EuroLinux repositories and/or other
repositories. If you add EuroLinux repositories the installer will automatically
download the newer version of the packages.

repos:

- URL: https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/certify-BaseOS/os  
  Name: BaseOSProd
- URL: https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/certify-AppStream/os  
  Name: AppStreamProd
- URL: https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/certify-PowerTools/os  
  Name: PowerToolsProd

For EuroLinux beta:

- URL: https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/certify-beta-BaseOS/os  
  Name: BaseOSBeta
- URL: https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/certify-beta-AppStream/os  
  Name: AppStreamBeta
- URL: https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/certify-beta-PowerTools/os  
  Name: PowerToolsBeta

!!! Warning "Limited support"
    **Network hybrid installation from ISO and public repositories are not our
    primary goal in QA process**. Packages in EuroLinux repositories are
    regulary updated and it might be impossible to install system from external
    source.  Depending on the state of the upstream we might not fix potential
    issue.

Below example configuration:


After changing the installation source you will have to confirm **Software
Selection**.

##### Software Selection

Software selection allows you to customize installation. You might choose from
base environemnt groups like Server with GUI, Workstation or Minmal. There is
also possiblity to install additonal packages groups.

#### Anaconda installer **SYSTEM** section:

- **Installation selection** allows to choose on which disk the system will be
  installed. If you want to have fully encrypted system you should enable
  encryption in this step.
- **KDUMP** allows to choose if KDUMP (Kernel crash dump collection mechanizm)
  should be enable.
- **Network & Hostname** - you can configure your system networking here
- **Security policy**  - you can setup additional openscap policy here

#### Anaconad installer **USER SETTINGS** section:

- **Root Password** - by default root user is disabled. If user is created in
  **User Creation** section, the root account could stay disabled
- **User Creation** section allow to create regular user in the system. If
  "Make this user administartor" option is check then root account could be disabled.


**After appling all necessary changes** installation could be start with "Begin
Installation" button.


### Disk partition recommended minimums

You need at least the following partitions

- `/boot` for Linux kernel and initramdisks - 1GB
- `/` (root partion) - at least 10 GB (very minimal system might use as little
  as 2GB - but it require extra attention and is not officialy supported by
  EuroLinux)

For UEFI efi system partiton is also required

- `/boot/efi`  - at least 100MB

If your storage allows it the following partition are also highly recommended:

- `swap` - 1GB or more depending on the system RAM and workload. Swap is also
  required for hibernation. Depending on the workload of the system it should
  be at least as spacious as system RAM.
- `/home` - at least 1GB - but in the most cases if `/` can be as big as 80GB
  then `/home/` usually takes the rest of the
  free space


### Installation in basic graphic mode

If there is problem loading/running your graphic card driver (it might results in black
screen / error message or graphic artiffacts). There is possiblity to install EuroLinux
in basic graphic mode.

To do so, on the welcome menu, choose `Troubleshooting`, and then "Install in
the basic graphics mode".

From this point the process is identical to standard installation with GUI.

### Installing EuroLinux in text mode

To install EuroLinux in the text mode:

- Boot EuroLinux ISO
- Press ++esc++ to stop installation options selection timeout
- Press ++tab++
- Add `inst.text` to the end of the kernel boot command line
- Press ++enter++

![TODO](TODO-IMAGE)

Before runnign installation you have to fill all necessary information
(represeneted as `!` in selection). It's good idea to refresh menu with
++r+enter++ command shortly after installer start.

![TODO](TODO-IMAGE)

After customization you are ready to start your installation

```
```

### Other possibilities

EuroLinux can also be installed in the following manner:

- Automated installation with kickstart file.
- Installation with PXE
- Installing via VNC

Please consult upstream documentation about this topics.

## Using EuroLinux

## Updating EuroLinux

## Submitting a Request for Change

We truly care. If ther is something that you belive that could/should be
changed about EuroLinux distribution and does not break compatiblity with
upstream project, then drop us ISSUIE on GitHub! All contributors are
exteremley welcome.


This repository allows you to stay in touch with Developers


## Submitting a Bug report

We decided that subbmiting bug report 

This repository allows you to stay in touch with Developers

