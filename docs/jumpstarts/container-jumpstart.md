# EuroLinux Containers JumpStart

Using EuroLinux container images is really easy.

You can choose to get OCI standard container image from two primary sources:

[docker hub](https://hub.docker.com/r/eurolinux/eurolinux-8) or [quay.io](https://quay.io/repository/eurolinux/eurolinux-8)

EuroLinux images can be run with any OCI standardized container runtimes such as runC (Docker/Moby project) or crun (Podman/Buildah/CRI-O).


To download the image:

Docker Hub:

```
docker pull eurolinux/eurolinux-8
```


Quay.IO:
```
podman pull quay.io/eurolinux/eurolinux-8
```

To run container, that will be removed after process exit, you might use:
```
docker run -rm -ti eurolinux/eurolinux-8
```

Inside container you can check system version
```
bash-4.4# cat /etc/el-release 
EuroLinux release 8.5 (Tirana)
```

To detach the container from the console add `-d` flag to docker run command.

```
docker run -rm -ti -d --name eurolinux eurolinux/eurolinux-8
```
then run:
```
docker exec -it eurolinux bash
```
to enter the console of a container.

If you want to use previous version of EuroLinux container you have to find desired version on [docker hub](https://hub.docker.com/r/eurolinux/eurolinux-8/tags) or [quay.io](https://quay.io/repository/eurolinux/eurolinux-8?tab=tags)
