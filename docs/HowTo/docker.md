# Docker

How to set up your Docker Community Edition installation.

## Introduction

### Use cases

Consider the following examples:

- You want a standardized runtime environment across production, QA and
  developer scenarios.
- You want all the runtime environment's specification in a single file, what
  is easy to manage through version control systems.
- You want the environment to be reproducible. After all, it's just a matter of
  building an image from the specification and once it's done it is already
self-documented on what steps were taken to cook the final image.
- You prefer a layered architecture and the ability to cache artifacts across
  several images and backup & restore the images easily.

### Why use Docker containers over virtual machines?

In short: Docker containers utilize Linux's capabilities such as cgroups and
namespaces to create an isolated environment and do not virtualize hardware.
Therefore, they are way more lightweight than virtual machines and can be
brought up in a large scale in a blink of an eye rather than waiting for a
single virtual machine to boot.

For more information, take a look at our blog entry on [the basics of
containerization](https://en.euro-linux.com/blog/the-basics-of-containerization/).

## System requirements

The following operating systems and architectures are covered by this guide:

- EuroLinux 8 on the x86_64 architecture.
- EuroLinux 7 on the x86_64 architecture.

Make sure the containers you want to run are of the same architecture as your
machine.

If you need support with installation on the EuroLinux releases this guide does
not cover, please create an appropriate ticket.

## Installation

### EuroLinux 8

The following steps are based on [Docker, Inc. official guide as of
2022.02.01](https://web.archive.org/web/20220201054013/https://docs.docker.com/engine/install/centos/).
We will just use the commands provided as snippets for a quick way of copying
& pasting one snippet for a successful installation.

The following snippet installs Docker on EuroLinux 8.5. Other releases may work
as well, but have not been tested. Once a new EuroLinux release is out, this
guide will be updated.

```
sudo yum remove -y docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
which yum-config-manager || sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install -y docker-ce docker-ce-cli containerd.io
sudo systemctl enable docker --now
```

### EuroLinux 7

EuroLinux provides their own builds of Docker for EuroLinux 7.

Please prepare your EuroMan credentials and enable the
`el-server-7-extras-x86_64` channel first, like so:

```
sudo rhn-channel -u "$el_euroman_user" -p "$el_euroman_password" -c el-server-7-extras-x86_64 -a
```

Then you are ready to install Docker:

```
sudo yum install -y docker
sudo systemctl enable docker --now
```

## What's next?

[EuroLinux provides several Docker images for you to
use](https://hub.docker.com/u/eurolinux). They are publicly available and free
of charge. Take a look at our entry [*EuroLinux docker images are now
available*](https://en.euro-linux.com/blog/eurolinux-docker-images-are-now-available/)
for more information. Additionally, [we provide a quick guide for having the
images up and running in no time](../jumpstarts/container-jumpstart.md).
