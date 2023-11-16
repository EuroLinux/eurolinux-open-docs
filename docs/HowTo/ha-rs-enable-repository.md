# How to install High Availability and Resilient Storage in EuroLinux 8

For EuroLinux 8.4, you should update the `el-release` package. The newer
version has `resilient-storage` and `high-availability` repositories saved in
the `/etc/yum.repos.d/certify.repo` file.

```
sudo yum update -y el-release
```

!!! info "'certify-' prefix"
    Since EuroLinux 8.6 the 'certify-' prefixes in repo URLs and names are no
    longer used. These URLs are and will be kept as the symbolic link for
    backward compatibility. The `certify.repo` file will be used for the whole
    EuroLinux 8 lifecycle.


If you cannot update the release package because the new `el-release` package errata
is not security-related, you might manually add the following to the
`/etc/yum.repos.d/certify.repo`

```
[high-availability]
name = EuroLinux High Availability
baseurl=https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/HighAvailability/os
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux8

[resilient-storage]
name = EuroLinux Resilient Storage
baseurl=https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/ResilientStorage/os
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux8
```

## Enabling repositories permanently

### Enabling High Availability and Resilient Storage repository manually

Use your favourite text editor and change `enabled=0` to `enabled=1` for
`high-availability` and `resilient-storage` repositories.

Before edit:
```
[resilient-storage]
name = EuroLinux Resilient Storage
baseurl=https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/ResilientStorage/os
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux8
```

After Edit:

```
[resilient-storage]
name = EuroLinux Resilient Storage
baseurl=https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/ResilientStorage/os
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux8
```

You should do the identical changes for high-availability repositories.

### Enabling High Availability and Resilient Storage repository with yum-config-manager

The `yum-config-manager` command is part of the `yum-utils` package. Firstly
let's install that package:

```bash
sudo yum install -y yum-utils
```

Then enable Resilient Storage and High Availability repository with the
following command:

```
sudo yum-config-manager --enable high-availability
sudo yum-config-manager --enable resilient-storage
```

## Installing HA and/or Resilient Storage

Both High Availability and Resilient Storage have rpm groups, so installing
them is trivial.

To install the High Availability add-on, invoke the following command:

```
sudo yum install -y @ha
```

To install the Resilient Storage add-on, invoke the following command:

```
sudo yum install -y @resilient-storage
```

## Basic HA configuration

### Configuring firewalld

Before configuring a firewall, it's appropriate to check if firewalld is actually
running. The standard `systemctl is-active` command is one of the options.

```
systemctl is-active firewalld
```

For a system that has firewalld started and enabled, you might use good
enough configuration with:

```
sudo firewall-cmd --permanent --add-service=high-availability
sudo firewall-cmd --reload
```

### Starting pcsd

After configuring a firewall, you can start and enable pcsd (PCS GUI and remote
configuration interface) with the following commands:
```
sudo systemctl start pcsd.service
sudo systemctl enable pcsd.service
```

To make a very basic test of the pcsd installation, we recommend setting
`hacluster` user password. As `root` user, you can, for example invoke:

```
# echo "secret-pass" | passwd hacluster --stdin
```

Then login into Pacemaker/Corosync configuration. Use the machine address on
port 2224 (example: `https://MACHINE_IP:2224`) in your browser. The
username is `hacluster` with password you set in previous step.

!!! warning "HTTPS Required"
    Web browser like Firefox and other programs like cURL will report
    "Connection reset by peer" or "The connection was reset" when connecting
    with cleartext HTTP.
    ```
    [root@test1 pcsd]# curl localhost:2224
    curl: (56) Recv failure: Connection reset by peer
    ```

From this point you can freely configure High Availability and Resilient
Storage. We recommend using upstream documentation provided in Additional Links
below.

## Additional links

- [Red Hat Documentation - Configuring and Managing High Availability Clusters](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_high_availability_clusters/index)
- [Red Hat Documentation - Configuring GFS2 File System](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_gfs2_file_systems/index)
