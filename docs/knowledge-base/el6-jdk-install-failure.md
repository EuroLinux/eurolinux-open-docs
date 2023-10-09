# Unable to install java-1.7.0-openjdk on EuroLinux 6

## Scenario

An attempt to install the latest version of `java-1.7.0-openjdk.x86_64` as part of the [EuroELS](https://en.euro-linux.com/eurolinux/euroels/) subscription fails with a message like:

```
Error in PRETRANS scriptlet in rpm package 1:java-1.7.0-openjdk-1.7.0.261-2.6.22.1.el6_10.x86_64
error: lua script failed: /usr/libexec/copy_jdk_configs.lua:272: attempt to index global 'file' (a nil value)
```

## Solution

The package `java-1.7.0-openjdk.x86_64` expects that the directory `/var/lib/rpm-state/` exists. Still, this might not be the case on every installation.

Please create this directory manually:

```
# mkdir /var/lib/rpm-state/
```
