# How to set up internal EuroLinux RPM mirror

This short how-to instructs how to set up your own **internal** EuroLinux mirror.
External (publicly available) mirrors should not be set up this way.

## System requirements

- Internet connection for sync server is required
- The firewall must allow connection to EuroLinux servers
- For each version of EuroLinux, you need about 80 GB of storage
- You have to install utilities like reposync and createrepo. The
  following command will work on an Enterprise Linux 7 and 8:
  ```bash
  # --skip-broken because depending on the version not all packages might be present
  sudo yum install -y createrepo_c createrepo yum-utils dnf-utils --skip-broken
  ```

## Mirroring EuroLinux 9

Making a local mirrors for EuroLinux 8 and EuroLinux 9 is simple because:

- repositories are open
- reposync can pull repository metadata, erratas, and modules files
  automatically.

!!! info "Use Enterprise Linux 9"
    These instructions have been tested to work properly on Enterprise
    Linux 9 and Enterprise Linux 8.

First, let's create the directory where mirroring configuration will reside:
```
sudo mkdir -p /etc/yum-mirror-config
```

Then, let's create configuration file for EuroLinux 9 mirroring
`/etc/yum-mirror-config/mirror_yum_el9.conf` with the contents:

```ini
[main]
cachedir=/var/cache/yum/mirror/$basearch/$releasever
keepcache=0
debuglevel=2
logfile=/var/log/mirror-yum-el9.log
plugins=1
exactarch=0
obsoletes=0
reposdir=/dev/null

[baseos]
name = EuroLinux BaseOS
baseurl=https://fbi.cdn.euro-linux.com/dist/eurolinux/server/9/$basearch/BaseOS/os
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux9
skip_if_unavailable=1

[appstream]
name = EuroLinux AppStream
baseurl=https://fbi.cdn.euro-linux.com/dist/eurolinux/server/9/$basearch/AppStream/os
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux9
skip_if_unavailable=1

[crb]
name = EuroLinux CRB
baseurl=https://fbi.cdn.euro-linux.com/dist/eurolinux/server/9/$basearch/CRB/os
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux9
skip_if_unavailable=1
```

Then invoke the command `reposync` with the following arguments:

```
reposync --downloadcomps --download-metadata -c /etc/yum-mirror-config/mirror_yum_el9.conf -p /repos
```

## Mirroring EuroLinux 8

Making a local mirror for EuroLinux 8 and EuroLinux 9 is simple because:

- repositories are open
- reposync can pull repository metadata, erratas, and modules files
  automatically.

!!! info "Use Enterprise Linux 8"
    These instructions have been tested to work properly on Enterprise
    Linux 8. While everything may work well, it's not recommended to use
    other versions.

First, let's create the file `/etc/yum-mirror-config/mirror_yum.conf`
with the contents:

```ini
[main]
cachedir=/var/cache/yum/mirror/$basearch/$releasever
keepcache=0
debuglevel=2
logfile=/var/log/mirror-yum.log
plugins=1
exactarch=0
obsoletes=0
reposdir=/dev/null

[baseos]
name = EuroLinux BaseOS
baseurl=https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/BaseOS/os
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux8
skip_if_unavailable=1

[appstream]
name = EuroLinux AppStream
baseurl=https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/AppStream/os
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux8
skip_if_unavailable=1

[powertools]
name = EuroLinux PowerTools
baseurl=https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/PowerTools/os
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux8
skip_if_unavailable=1
```

Then invoke the command `reposync` with the following arguments:

```
reposync --downloadcomps --download-metadata -c /etc/yum-mirror-config/mirror_yum.conf -p /repos
```

## Mirroring EuroLinux 7

!!! info "Use Enterprise Linux 7"
    These instructions have been tested to work properly on Enterprise
    Linux 7. While everything may work well, it's not recommended to use
    other versions.

### The official way

**EuroLinux 7 is not open-core**; therefore, only organizations with a proper
license (EuroMan or Golden Key) can mirror it freely.

!!! information "We know"
    We are well aware that it is possible to mirror repos even with a single
    license. You can read about that below.

The official way to mirror EuroLinux repositories is the following:

- You need a proper subscription, like EuroMan or Golden Key
- EuroLinux engineer will provide you with SSL certificates that we will name
  `repo.key` and `repo.crt` and CA that we will name `ca.crt`

Create the directory `/etc/yum-mirror-config/`.
With the repokeys residing in that directory, create the file
`/etc/yum-mirror-config/mirror_yum.conf` with the contents:

```ini
[main]
cachedir=/var/cache/yum/mirror/$basearch/$releasever
keepcache=0
debuglevel=2
logfile=/var/log/mirror-yum.log
exactarch=0
obsoletes=0
gpgcheck=0
plugins=0
reposdir=/dev/null

[eurolinux-os-7]
name=el7_x86_64_os
baseurl=https://cdn.euro-linux.com/dist/eurolinux/server/7/x86_64/os/
sslclientkey=/etc/yum-mirror-config/repo.key
sslclientcrt=/etc/yum-mirror-config/repo.crt
sslcacert=/etc/yum-mirror-config/ca.crt

[eurolinux-updates-7]
name=el7_x86_64_updates
baseurl=https://cdn.euro-linux.com/dist/eurolinux/server/7/x86_64/updates/
sslclientkey=/etc/yum-mirror-config/repo.key
sslclientcrt=/etc/yum-mirror-config/repo.crt
sslcacert=/etc/yum-mirror-config/ca.crt
```

Then invoke the command `reposync` with the following arguments:

```
reposync -d -m --download-metadata -c /etc/yum-mirror-config/mirror_yum.conf -p /repos
```

When the download finishes, the next step is to create repodata and enable
groups.

```bash
cd /repos/eurolinux-os-7/; createrepo . -g comps.xml
cd /repos/eurolinux-updates-7/; createrepo . -g comps.xml
```

!!! info Erratas
    Enabling updateinfo (erratas information) is a little bit tricky, because
    firstly you have to find the newest updateinfo, unpack it, then invoke
    modifyrepo script. It can be automated with the script below.

```bash
REPO_DIR=/repos/eurolinux-os-7/
unset -v LAST_UI
# finding the newest file
for file in "$REPO_DIR"/*updateinfo.xml.gz; do
  [[ "$file" -nt "$LAST_UI" ]] && LAST_UI=$file
done
# unpacking to updateinfo.xml file
sudo gunzip -c "$LAST_UI"  > "$REPO_DIR/updateinfo.xml"
# Depending on the system - some has modifrepo.py script some has "normal" command
/usr/share/createrepo/modifyrepo.py "$REPO_DIR/updateinfo.xml"  "$REPO_DIR/repodata" || modifyrepo "$REPO_DIR/updateinfo.xml"  "$REPO_DIR/repodata"

REPO_DIR=/repos/eurolinux-updates-7/
unset -v LAST_UI
for file in "$REPO_DIR"/*updateinfo.xml.gz; do
  [[ $file -nt $LAST_UI ]] && LAST_UI=$file
done
sudo gunzip -c "$LAST_UI"  > "$REPO_DIR/updateinfo.xml"
# Depending on the system - some has modifrepo.py script some has "normal" command
/usr/share/createrepo/modifyrepo.py "$REPO_DIR/updateinfo.xml"  "$REPO_DIR/repodata" || modifyrepo "$REPO_DIR/updateinfo.xml"  "$REPO_DIR/repodata"
```

### The unsupported way

There is also the possibility to mirror EuroLinux repositories even with a
single or even test subscription.

!!! warning "True Product – Real Support – Fair Price"
    We are faithful to our values. We also know that it's always possible to
    cheat and not play fair. Please be aware that during support inqury, we
    might check if your system is registered and supported. To this day, we
    always had pleasure to work with honest companies - please don't ruin that.

!!! danger "Mirroring other distros"
    The instruction allows cloning other distros, including paid ones. If you
    want to mirror paid Linux distribution, note that this might breach the
    license/license agreement.

You can mirror EuroLinux or other Enterprise Linux repositories with the
following step:

- Register your system with `rhn_register` command for EuroLinux or another way
  to mirror another system repositories.

Then run the following snippet as root:

```bash
reposync -d -m --download-metadata --plugins -r el-server-7-x86_64 -p /repos/
# recreating repodata and updateinfo
REPO_DIR=/repos/el-server-7-x86_64/
cd /repos/el-server-7-x86_64/; createrepo . -g comps.xml
unset -v LAST_UI
for file in "$REPO_DIR"/*updateinfo.xml.gz; do
  [[ $file -nt $LAST_UI ]] && LAST_UI=$file
done
sudo gunzip -c "$LAST_UI"  > "$REPO_DIR/updateinfo.xml"
# Depending on the system - some has modifrepo.py script some has "normal" command
/usr/share/createrepo/modifyrepo.py "$REPO_DIR/updateinfo.xml"  "$REPO_DIR/repodata" || modifyrepo "$REPO_DIR/updateinfo.xml"  "$REPO_DIR/repodata"
```

## Mirroring EuroLinux 6 ELS

First, register your system to EuroLinux EuroMan with the [migration scripts](https://github.com/EuroLinux/eurolinux-migration-scripts/tree/el6-only-switch-repos) - use the `el6-only-switch-repos` branch for this.

Once the system has been registered and is receiving EL6 ELS updates, you can mirror the ELS packages with the following commands. Run them as root:

```
reposync -d -m --download-metadata --plugins -r els-6-x86_64 -p /repos/
# recreating repodata and updateinfo
REPO_DIR=/repos/els-6-x86_64/
cd /repos/els-6-x86_64/; createrepo . -g comps.xml
unset -v LAST_UI
for file in "$REPO_DIR"/*updateinfo.xml.gz; do
  [[ $file -nt $LAST_UI ]] && LAST_UI=$file
done
sudo gunzip -c "$LAST_UI"  > "$REPO_DIR/updateinfo.xml"
# Depending on the system - some has modifrepo.py script some has "normal" command
/usr/share/createrepo/modifyrepo.py "$REPO_DIR/updateinfo.xml"  "$REPO_DIR/repodata" || modifyrepo "$REPO_DIR/updateinfo.xml"  "$REPO_DIR/repodata"
```
