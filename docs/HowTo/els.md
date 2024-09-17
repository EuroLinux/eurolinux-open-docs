# Migrate to EuroELS

This guide is about how to switch your repositories to the ones provided by EuroLinux Extended Life Support.

## Introduction

Enterprise Linuxes 6 ended their life a few years ago. Still, security updates can be provided by several vendors for a fee. That's where EuroELS comes in. You can extend the lifecycle of your Enterprise Linux up to the half of 2024.

## How to migrate

First, make sure that your system is up to date. It should be CentOS 6.10 (minor version 10).

```
su
yum update -y
```

Second, in accordance with good practice, we recommend backing up your machine.

Please download the migration script available at this location:

https://github.com/EuroLinux/eurolinux-migration-scripts.git 

```
wget https://github.com/EuroLinux/eurolinux-migration-scripts/archive/refs/heads/el6-only-switch-repos.zip
```

Please unpack the downloaded file:

```
unzip el6-only-switch-repos.zip
```

and navigate to the script's directory:

```
cd eurolinux-migration-scripts-el6-only-switch-repos
```

To start the switching process, just run the script with administrator privileges:

```
./migrate2eurolinux.sh
```

Once the command is executed, we'll get a recommendation to make a backup. Type YES to make the script continue.

The script will ask us about our EuroMan credentials. We provide our login and password when asked.

The repository switch has completed successfully. We can now update our Enterprise Linux 6 with the command:

```
yum update -y
```

