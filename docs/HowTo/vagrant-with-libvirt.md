# Vagrant with libvirt

## Introduction

This guide covers the installation of libvirt and related tools along with the
Vagrant plugin that allows using libvirt as a provider. This has been tested on
a clean installation of EuroLinux 8.4 - only Vagrant has been installed already
as described in [Vagrant jumpstart](../jumpstarts/vagrant-jumpstart.md).  

### Terminology

- **QEMU** - a generic machine emulator
- **KVM** - a virtualisation solution that is native to Linux. Used by QEMU to
  achieve near native performances by executing the guest code directly on
  the host CPU
- **libvirt** - a management suite for several hypervisors

While libvirt can manage many virtualisation solutions, in the context of this
document *libvirt* refers to: *QEMU with KVM managed by libvirt*.  

### Why prefer libvirt over providers such as VirtualBox?

As mentioned, KVM is a native virtualisation solution to Linux. This means a
[significant performance
boost](https://web.archive.org/web/20210119220104/https://www.phoronix.com/scan.php?page=article&item=virtualbox-60-kvm&num=1)
when compared to other providers at the slight cost of portability - if you run
Linux only, then this is your solution of choice!  

If you have never used libvirt before and just heard about it in this how-to,
there are several goodies worth mentioning. As an example unrelated to Vagrant:
Virt-Manager allows you to get a similar GUI experience out of KVM as that of
e.g. VirtualBox, it is fully Free Software (no worrying about licensing
shenanigans) and is more modular - in fact, there is a [libvirt VirtualBox
driver](https://libvirt.org/drvvbox.html) out there.  

## Install the plugin

Normally one would invoke a single command: `vagrant plugin install
vagrant-libvirt` and the plugin would work well out-of-the-box. This is not the
case for Linux distributions from the Enterprise Linux family and [Upstream is
aware of that](https://github.com/hashicorp/vagrant/issues/11020), but as of
today (2021.09.04) it doesn't appear to be resolved.  

Because of that, you'll need to build additional components and use them with
your Vagrant installation. The following procedure covers all of this and has
been tested to work well with EuroLinux 8.4.  

There are several references to CentOS in the procedure. The reason is that
EuroLinux-maintained source RPMs will be provided soon in Q4 and once they're
available, the procedure will be updated.  

Use these commands:  

```
[ "$(command -v vagrant)" ] || \
( read -p "Install Vagrant first before running the following commands" \
  && exit 1 )

sudo dnf groupinstall "Development Tools" "Virtualization Host" -y
sudo dnf install cmake libvirt-devel ruby-devel -y

git clone https://git.centos.org/centos-git-common
export PATH=$(readlink -f ./centos-git-common):$PATH

git clone https://git.centos.org/rpms/krb5
cd krb5
git checkout imports/c8s/krb5-1.18.2-8.el8
into_srpm.sh -d c8s
cd SRPMS
rpm2cpio krb5-1.18.2-8c8s.src.rpm | cpio -imdV
tar xf krb5-1.18.2.tar.gz
cd krb5-1.18.2/src/
./configure
make
sudo cp -P lib/crypto/libk5crypto.* /opt/vagrant/embedded/lib64/

cd ../../../../

git clone https://git.centos.org/rpms/libssh
cd libssh
git checkout imports/c8s/libssh-0.9.4-1.el8
into_srpm.sh -d c8s
cd SRPMS
rpm2cpio libssh-0.9.4-1c8s.src.rpm | cpio -imdV
tar xf libssh-0.9.4.tar.xz
mkdir build
cd build
cmake ../libssh-0.9.4 -DOPENSSL_ROOT_DIR=/opt/vagrant/embedded/
make
sudo cp lib/libssh* /opt/vagrant/embedded/lib64

cd ../../../

vagrant plugin install vagrant-libvirt
sudo usermod -a -G libvirt $USER
```

Next, log out of your graphical session and log in again. From now on you
should be able to run Vagrant boxes with libvirt if all requirements have been
satisfied, e.g. you don't have any other providers enabled (an equivalent of
deploying this guide on a clean installation).  

## Additional resources

- The [plugin's repository](https://github.com/vagrant-libvirt/vagrant-libvirt)
- Websites of provider-related projects: [KVM](http://linux-kvm.org/),
  [libvirt](http://libvirt.org/), [QEMU](http://qemu.org),
  [Virt-Manager](http://virt-manager.org/)
