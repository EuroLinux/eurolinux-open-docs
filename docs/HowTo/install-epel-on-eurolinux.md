# How to Install and Enable EPEL repository on EuroLinux 8

EPEL (Extra Packages for Enterprise Linux) repository is one of the most
popular third-party repositories for the Enterprise Linux family. From 4th
November EuroLinux team included the original `epel-release` package from EPEL in
the BaseOS repo. It was the most voted **small quality of life change** during our
first community meeting. The package is resigned with a EuroLinux GPG key, so
there is no need to accept an external key to install this particular package.


The package version will be checked and updated if necessary **during minor
releases**.


!!! info "EPEL is an external repository that is community supported"
    As company, we cannot provide proper care in terms of the security, quality,
    support and lifecycle standards on a third-party repository. Nevertheless, the
    EPEL repository has a great history of community support.


## Installing EPEL on EuroLinux

Installing EPEL on EuroLinux 8 and EuroLinux 7 is as simple as:

```bash
sudo yum install -y epel-release
```

!!! info "EPEL is enabled by default"
    The base EPEL repository is enabled by default for modular (EuroLinux 8/9)
    and not-modular packages. You can enable debuginfo and source packages in
    respective `/etc/yum.repos.d/epel*.repo` file.

From this moment, you can install then use all EPEL goodies like `htop`, `wine` or
`createrepo`.
