# Migrate to EuroELS

This guide is about how to switch your repositories to the ones provided by EuroLinux Extended Life Support.

## Introduction

Enterprise Linuxes 6 ended their life a few years ago. Still, security updates can be provided by several vendors for a fee. That's where EuroELS comes in. You can extend the lifecycle of your Enterprise Linux up to the half of 2024.

## How to migrate

Please download the migration script available at this location:
https://github.com/EuroLinux/eurolinux-migration-scripts.git 
```
git clone https://github.com/EuroLinux/eurolinux-migration-scripts.git 
```

Once the download is complete, we navigate to the script's directory:
```
cd eurolinux-migration-scripts
```

We switch the branch to `el6-only-switch-repos`:
```
git checkout el6-only-switch-repos
```

And run the script with superuser privileges:
```
sudo ./migrate2eurolinux.sh
```

Once the command is executed, we'll get a recommendation to make a backup. Type `YES` to make the script continue.

The script will ask us about our EuroMan credentials. We provide our login and password when asked.

The repository switch has completed successfully. We can now update our Enterprise Linux 6 with the command:
```
sudo yum update -y
```

