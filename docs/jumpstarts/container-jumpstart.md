# EuroLinux Containers JumpStart

## Basics

Using EuroLinux container images is easy. We provide multiple containers and
their versions. The most important are base images for EuroLinux 7 (with FBI
[Free Base Image] repository) and EuroLinux 8 (with complete repositories -
EuroLinux 8 is freely available). All EuroLinux container images are OCI
standard container images. You can download them from two primary sources:

- [Docker Hub - EuroLinux 8](https://hub.docker.com/r/eurolinux/eurolinux-8)
- [Docker Hub - EuroLinux 7](https://hub.docker.com/r/eurolinux/eurolinux-7)
- [Quay - EuroLinux 8](https://quay.io/repository/eurolinux/eurolinux-8)
- [Quay - EuroLinux 7](https://quay.io/repository/eurolinux/eurolinux-7)


EuroLinux images can be run with any OCI standardized container runtimes such
as runC (Docker/Moby project) or crun (Podman/Buildah/CRI-O).


**To download the image:**

Docker Hub:
``` bash
docker pull eurolinux/eurolinux-8
```

Quay.IO:
```
podman pull quay.io/eurolinux/eurolinux-8
```

To run a container that will be removed after process exit. You might use:
```
docker run -rm -ti eurolinux/eurolinux-8
```

Inside the container, you can check the system version
```
bash-4.4# cat /etc/el-release 
EuroLinux release 8.5 (Tirana)
```

To detach the container from the console, add `-d` flag to docker/podman run command.

```
docker run -rm -ti -d --name eurolinux eurolinux/eurolinux-8
```
then run:
```
docker exec -it eurolinux bash
```
to enter the console of a container.

## Other containers

As a company, we create and support multiple containers, including other
Enterprise Linux distributions. For example, we provide:

- [Docker Hub - AlmaLinux](https://hub.docker.com/r/eurolinux/almalinux-8)
- [Docker Hub - Rocky](https://hub.docker.com/r/eurolinux/rocky-8)
- [Quay - AlmaLinux](https://quay.io/repository/eurolinux/almalinux-8)
- [Quay - Rocky](https://quay.io/repository/eurolinux/rocky-8)

Other containers that we build are:

- Oracle Linux
- Scientific Linux
- CentOS and CentOS stream

You can visit our organization page on the [Docker
Hub](https://hub.docker.com/u/eurolinux) to find all containers that we
officially support.

## Older Versions

If you want to use the previous version of the EuroLinux container, you must
find the desired tag. Example for EuroLinux 8:

- [Docker Hub - EuroLinux 8 - tags](https://hub.docker.com/r/eurolinux/eurolinux-8/tags)
- [Quay - EuroLinux  - tags](https://quay.io/repository/eurolinux/eurolinux-8?tab=tags)

## Request for Change/Comment and Bug report repository

You can request a change, leave a comment or report a bug in this [EuroLinux
containers RFC](https://github.com/EuroLinux/containers-rfc) repository.:w

