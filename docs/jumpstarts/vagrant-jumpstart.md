# Vagrant
## Introduction
Ever wanted to create a development environment that is guaranteed to work
flawlessly on several developers' workstations without the *It works on my
machine!* excuses?  
Well, now you can! Just get Vagrant, write your specification and share it with
coworkers!  

## Requirements

Vagrant will be managing the virtual machines of the [provider of your
choice](https://www.vagrantup.com/docs/providers). If you find out it's not
listed, consider checking if there's a plugin-based implementation.  
Make sure you have a supported provider installed - we'll be using VirtualBox
in this guide. You can use our [VirtualBox installation
guide](../HowTo/virtualbox.md) as a reference.  

## Installation on EuroLinux 8

Simply run these commands and you're ready to go:  

```
sudo dnf config-manager \
  --add-repo=https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo dnf install vagrant -y
```

## EuroLinux Boxes

EuroLinux Vagrant images are available at:
https://app.vagrantup.com/eurolinux-vagrant  
Here's a basic box creation procedure in a separate directory:  

```
mkdir el8-vagrant
cd el8-vagrant/
vagrant init eurolinux-vagrant/eurolinux-8
vagrant up
vagrant ssh
```
You should now be connected to the box and able to perform your desired
operations in it.  
Go ahead, play around, install your favourite developer tools, build an
awesome, advanced, multi-component application and be sure everyone's able to
deploy it on their workstations without any hassle!  

## Vagrantfile
Once you've ran the commands above, you'll have a *Vagrantfile* in the
*el8-vagrant* directory.  
Take a look, what's inside. You'll find several parameters that you can tweak,
e.g. the virtual machine's memory or the amount of CPUs. Adapt them to your
work and play around if they can be reduced once you know your applications
resource requirements.  
For additional information feel free to check out [Upstream's Vagrantfile
reference](https://www.vagrantup.com/docs/vagrantfile).  
