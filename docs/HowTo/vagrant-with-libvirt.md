# Vagrant with libvirt

## Introduction

This guide covers the installation of libvirt and related tools along with the
Vagrant plugin that allows using libvirt as a provider. This has been tested on
a clean installation of EuroLinux 8.4 - only Vagrant has been installed already
as described in [Vagrant jumpstart](../jumpstarts/vagrant-jumpstart.md).  

### Why prefer libvirt over providers such as VirtualBox?

KVM is a native virtualisation solution to Linux. This means a significant
performance boost when compared to other providers at the slight cost of
portability - if you only run Linux, then this is your solution of choice!  

The performance boost can be easily seen in
[tests](https://www.phoronix.com/scan.php?page=article&item=virtualbox-60-kvm&num=1)
- it achieves near native performances by executing the guest code directly on
the host CPU. This requires that both the host and guest machine use the same
architecture of x86, PowerPC or S390.  

Virt-Manager allows you can get a similar GUI experience out of KVM (among
others) as that of e.g. VirtualBox, it is fully Free Software (no worrying
about licensing shenanigans) and is more modular - you get several components
that integrate well with each other rather than a one-size-fits all solution.  

## Building the plugin



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

then log out of your graphical session and log in again.


