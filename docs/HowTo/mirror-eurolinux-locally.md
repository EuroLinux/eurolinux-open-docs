# How to set up EuroLinux RPM internal mirror

This short how-to instructs how to set up your own **internal** EuroLinux mirror.
External (publicly available) mirrors should not be set up this way.

## System requirement

- Internet connection for sync server is required
- The firewall must allow connection to EuroLinux servers
- For each version of EuroLinux, you need about 80 GB of storage
- You have to install utilities like reposync and createrepo. The following command will work on Enterprise Linux 7 and 8:
  ```bash
  # --skip-broken because not all packages might be present
  sudo yum install -y createrepo_c createrepo yum-utils dnf-utils --skip-broken
  ```

## Mirroring EuroLinux 7 - the official way

**EuroLinux 7 is not open-core**; therefore, only organizations with a proper
license (EuroMan or Gold Key) can mirror it freely.

!!! information "We know"
    We are well aware that it is possible to mirror repos even with a single
    license. You can read about that below.

The official way to mirror EuroLinux repositories is the following:

- You need a proper subscription, like EuroMan or Gold Key
- EuroLinux engineer will provide you with SSL certificates that we will name
  `repo.key` and `repo.crt` and CA that we will name `ca.crt`


With repokeys residing in `/etc/yum-mirror-config/` directory create the
following yum.config - example below is for EuroLinux 7 x86_64


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

After saving this file to `/etc/yum-mirror-config/mirror_yum.conf` you might download the
repositories to the `/repos` directory with the following command.
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

This short manual concludes mirroring EuroLinux locally.

## Mirroring EuroLinux 7 **unsupported way**

There is also the possibility to mirror EuroLinux repositories even with a
single or even test subscription.

!!! warning "True Product – Real Support – Fair Price"
    We are faithful to our values. We also know that it's always possible to
    cheat and not play fair. Please be aware that during support inqury, we
    might check if your system is registered and supported. To this day, we
    always had pleasure to work with honest companies - please don't ruin that.

!!! danger "Mirroring other distros"
    This instruction allows cloning other distros, including paid ones. If you
    want to mirror paid Linux distribution, note that this might breach the
    license/license agreement and endanger your organization.

After this short warning, you can mirror EuroLinux or other Enterprise Linux
repositories with the following step:

- Register your system with `rhn_register` command for EuroLinux or another way
  to mirror another system repositories.

Then run the reposync with proper options (example for EuroLinux 7):

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

## EuroLinux 8

!!! info "Use Enterprise Linux 8"
    The instructions below work on Enterprise Linux 8. Because EuroLinux 8 has
    modular repositories and it is **not supported** to use `reposync` on the
    older system releases.

Making a local mirror for EuroLinux 8 is much simpler because:

- repositories are open
- reposync can pull repository metadata, erratas, and modules files
  automatically.

First - let's create `/etc/yum-mirror-config/mirror_yum.conf`

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

[certify-baseos]
name = EuroLinux certify BaseOS
baseurl=https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/certify-BaseOS/os
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux8
skip_if_unavailable=1

[certify-appstream]
name = EuroLinux certify AppStream
baseurl=https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/certify-AppStream/os
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux8
skip_if_unavailable=1

[certify-powertools]
name = EuroLinux certify PowerTools
baseurl=https://fbi.cdn.euro-linux.com/dist/eurolinux/server/8/$basearch/certify-PowerTools/os
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux8
skip_if_unavailable=1
```

Then you can invoke the `reposync` command with the following arguments:
```
reposync --downloadcomps --download-metadata  -c /etc/yum-mirror-config/mirror_yum.conf  -p /repos
```

!!! note "Mirror using Enterprise Linux 8"
    If you use Enterprise Linux 8 and mirror Enterprise Linux 7 repositories,
    you don't have to rebuild repositories metadata and updateinfo files.
