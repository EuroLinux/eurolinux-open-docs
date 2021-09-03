# Vagrant

## Introduction

Ever wanted to create a development environment that is guaranteed to
work flawlessly on several developers' workstations without the *It
works on my machine!* excuses?  
Well, now you can! Just get Vagrant, write your specification and share
it with coworkers!  

## Requirements

Vagrant will be managing the virtual machines of the [backend provider
of your choice](https://www.vagrantup.com/docs/providers). If you find
out it's not listed, consider checking if there's a plugin-based
implementation.  
Make sure you have a supported provider installed - we'll be using
VirtualBox in this guide. You can use our [VirtualBox installation
guide](../HowTo/virtualbox.md) as a reference.  

## Installation on EuroLinux 8

Simply run these commands and you're ready to go:  

```
sudo dnf config-manager \
  --add-repo=https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo dnf install vagrant -y
```

## EuroLinux Boxes

A *box* is a format that defines: an image of an operating system with
preinstalled software, a provider for that image and its version - it's
a ready-made appliance for that provider to run.  
This appliance works the same across people's workstations, which most
likely will have differences in configuration and potentially different
providers or even operating systems. That is as long as they use a
provider, which the box is built for.  

EuroLinux Vagrant boxes are available at:
[https://app.vagrantup.com/eurolinux-vagrant](https://app.vagrantup.com/eurolinux-vagrant)  
Let's use the box `eurolinux-vagrant/eurolinux-8` as an example.  

### Box details

See the [details of the
box](https://app.vagrantup.com/eurolinux-vagrant/boxes/eurolinux-8) -
multiple providers, that the box has been built for, are listed along
with the box versions and build dates. When writing your specification,
you'll be able to choose from them as you wish.

### Run the box

Here's a basic procedure for running a Vagrant environment (a virtual
machine, which uses our box) in a separate directory:  

```
mkdir el8-vagrant
cd el8-vagrant/
vagrant init eurolinux-vagrant/eurolinux-8
vagrant up
vagrant ssh
```

You should now be connected to the machine and able to perform your
desired operations inside it.  
Go ahead, play around, install your favourite developer tools, build an
awesome, advanced, multi-component application and be sure everyone's
able to deploy it on their workstations without any hassle!  

## What about the specification mentioned earlier? - Vagrantfile

Once you've ran the commands above, you'll have a *Vagrantfile* in the
*el8-vagrant* directory.  
Take a look, what's inside. You'll be greeted with an introduction,
references and lots of common options along with comments explaining
them:  

```
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.
```

As you've already ran the box as explained in the previous section, you
can see that there's no **necessity** to tweak anything inside
Vagrantfile. The parameters that you can tweak, e.g. the virtual
machine's memory or the amount of CPUs you should adapt to your work -
e.g. if you need additional resources for developing/running your
application, go ahead and increase them. Once that's done, check out if
they can be reduced once you know your software resource requirements.

Depending on the task you want to achieve, whether it be a ready-made
appliance **or** a base virtual system that gets provisioned with your
application and its dependencies **or** something else, that's when
changes to the specification must be made - e.g. the additional
provisioning procedure shall be written.

As the comment quoted above says, refer to the comments the Vagrantfile
provided for common options explanation. Once you know their purpose,
try them out! Get comfortable with them and read [Upstream's 
documentation](https://docs.vagrantup.com/) for additional info, tips
and more advanced, cool possibilities - such as a [multi-machine 
infrastructure](https://www.vagrantup.com/docs/multi-machine) defined in
a single Vagrantfile.

## Additional resources

- Upstream's [official website](https://www.vagrantup.com/)
- [Discover Vagrant Boxes](https://app.vagrantup.com/boxes/search?&q=eurolinux)
  \- using EuroLinux as an example
- Upstream's [online documentation](https://docs.vagrantup.com.), worth
  mentioning once more
