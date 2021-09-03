# Vagrant with libvirt

## Introduction

This guide covers the installation of libvirt and related tools along with the
Vagrant plugin that allows using libvirt as a provider. This has been tested on
a clean installation of EuroLinux 8.4 - only Vagrant has been installed already
as described in [Vagrant jumpstart](../jumpstarts/vagrant-jumpstart.md).  

### Why prefer libvirt over providers such as VirtualBox?



## Building the plugin



```
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


