# EuroLinux Vault and Archiving Policy

## Vault - vault.cdn.euro-linux.com

The idea of the vault is simple - it stores older or archived versions of the
software packages (mostly RPMs) and allow to create and maintain the
environment that requires old versions of the packages, specific libraries or
cannot or even must not be updated.

!!! warning "Warning! Security considerations"
    Due to their nature, the old versions of the software contain security
    vulnerabilities. Many of them have ready and easy to use exploits. Using
    unsupported versions of software is a dangerous practice, especially
    when system is running in the public networks.

## Vault's initial and last version for minor releases

From version 8.6 and 9.0 we decided that we will provide two versions for each
minor release (minor release is for example EuroLinux 8.6 -> 8.7 -> 8.8), of the
vault. The first one is the initial state of the release, the second one has
'-last' and it's the last snapshot before a minor new release. This idea
originated from community feedback. For example:

- [https://vault.cdn.euro-linux.com/legacy/eurolinux/8/8.6/](https://vault.cdn.euro-linux.com/legacy/eurolinux/8/8.6/)
  contains the initial state
- [https://vault.cdn.euro-linux.com/legacy/eurolinux/8/8.6-last/](https://vault.cdn.euro-linux.com/legacy/eurolinux/8/8.6-last/)
  contains the last snapshot of EuroLinux 8.6 before 8.7 reached GA (General Availability)

If there is no version with `-last` suffix it means that this version is
a newest version.


## EuroLinux sources on vault.cdn.euro-linux.com

We used to provide sources for EuroLinux on GitHub. Unfortunately, due to
GitHub's limitations, it wasn't as feasible in long run. Each source needed to
be repacked on the client system and source wasn't signed.


We decided to go with a friendlier and more standarized direction after
receiving feedback from the community and clients. Since version 8 we deliver
the sources as .src.rpms. It's a better solution in many ways, and the
advantages include:

- All source packages (sources used to build EuroLinux and other Enterprise
  Linux distributions) are securely signed cryptographically 
- The src.rpm is native format for distributions that leverage RPMs. It allows
  to use common toolchains like mock, rpmbuild, EuroLinux Gaia, Open Build
  Service, Koji and much more
- It's easier to mirror repositories
- It's also easier to maintain


The source can be found at
[https://vault.cdn.euro-linux.com/sources/eurolinux/](https://vault.cdn.euro-linux.com/sources/eurolinux/).


## Bug tracker

If you encounter any missing sources or problem with vault, please don't
hesitate to contact as via e-mail (support[at]euro-linux.com) or fill bug in
our [distro bug
tracker](https://github.com/EuroLinux/eurolinux-distro-bugs-and-rfc). If you
are our client you can contact us on the support site or with sale
representative.
