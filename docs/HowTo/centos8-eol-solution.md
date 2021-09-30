# CentOS 8 End of Life - solution

## Introduction

This how-to provides a solution to the problem of CentOS 8 running out of
support.  
With the end of 2021, CentOS will end its life in its current form
and will start functioning as CentOS Stream, a development branch for Red Hat®
Enterprise Linux®. As a result, it will stop receiving proven, stable updates
and its use, especially in production environments, will become risky. This is
a very serious problem for many companies and individuals around the world. So
there was an urgent need to find a new source of updates for CentOS in order
to keep it in the infrastructure. A complete solution to this problem is
support switching, that is, pointing to a new repository from which CentOS
will be downloading stable updates. Such a solution is offered by EuroLinux.
It is worth mentioning that both CentOS and RHEL and EuroLinux are systems
built on the same source code, so they provide the same functionality. They
differ mainly in branding.

The operation of switching support is simple, safe and completely reversible.
What is very important, it requires neither reinstallation of the system nor
the applications installed on it. The process consists of switching the
repository, installing the el-release package and updating the system. After
the switch, CentOS will still be usable, even in production environments. 

All resources used in this tutorial can be found in the [additional
resources section](#additional-resources).

## The solution

A project named *eurolinux-migration-scripts* has been created. It contains
among others a script that will take care of the support switching operation
automatically. Here we describe, how to perform the switch successfully.

### Preparations

It's vital that the system be updated to the newest release. Use the following
command:

```bash
sudo yum update -y
```

### Running the migration script

Then download the [latest production-ready
release](https://github.com/EuroLinux/eurolinux-open-docs/archive/refs/heads/master.zip)
of the project containing the migration script. Unpack the release, visit the
unpacked directory and run the script - usually this will require
right-clicking in the current directory, using the 'Open in Terminal' option
and running this command:

```bash
sudo bash migrate2eurolinux.sh
```

Refer to the project's README for additional commands.

### After the switch

Once the support switching operation has finished, it's recommended to reboot
your system:

```bash
sudo reboot
```

Once the system has rebooted, the migration process can be considered
complete.  
In order to quickly verify that it was carried out successfully, we
can check the distribution description:

```bash
cat /etc/el-release
```

As a result we should get a response about the distribution and latest
EuroLinux version.

## Conclusion

As you can see, the migration is quick and seamless. You can switch the
repository for RHEL, Oracle Linux, AlmaLinux, and Rocky Linux the same way.
In each case, the process will look almost identical.

If you have any questions or concerns, please submit them to the migration
script repository linked in the [additional
resources section](#additional-resources). Thank you.

## Additional resources

- [EuroLinux migration scripts GitHub repository](https://github.com/EuroLinux/eurolinux-migration-scripts)
- [Latest production-ready
release of the project](https://github.com/EuroLinux/eurolinux-open-docs/archive/refs/heads/master.zip)
