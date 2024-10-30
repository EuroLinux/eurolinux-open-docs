# Using EuroLinux 7 from vaulted repositories

## EuroLinux 7 EOL 2024-06-30

EuroLinux 7 reached its end of life on 2024-06-30. It means no further updates,
including security updates, will be available. We strongly recommend upgrading
to EuroLinux 8 or later as soon as possible. However, you may still need to use
the older version for development, legacy, and compatibility reasons.

## EuroLinux 7 vault

Using the following gist is a straightforward way to access the EuroLinux 7
vault:

<script src="https://gist.github.com/AlexBaranowski/4d1aab197164ec55e200c00ceb6884db.js"></script>


Or manually add the following to `/etc/yum.repos.d/eurolinux-7-vault-repos.repo`:

```ini
[eurolinux7-base]
name=Eurolinux 7 Base Vault
baseurl=https://vault.cdn.euro-linux.com/legacy/eurolinux/7/7.9/os/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux7

[eurolinux7-updates]
name=Eurolinux 7 Updates Vault
baseurl=https://vault.cdn.euro-linux.com/legacy/eurolinux/7/7.9/updates/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux7
```


The GPG key should be available in the system, but if it is not, you can add it
with the following:


```bash
curl -o /etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux7 https://fbi.cdn.euro-linux.com/security/RPM-GPG-KEY-eurolinux7
```

## EuroLinux 7.8 and 7.7

As a bonus, you could also use even older versions of EuroLinux 7.8 and 7.7,
which contain only updates up to the last minor release of the given version.
Sample content of the `.repo` files are below:

For EuroLinux 7.8:
```ini
[eurolinux7-base]
name=Eurolinux 7.8 Base Vault
baseurl=https://vault.cdn.euro-linux.com/legacy/eurolinux/7/7.8/os/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux7

[eurolinux7-updates]
name=Eurolinux 7.8 Updates Vault
baseurl=https://vault.cdn.euro-linux.com/legacy/eurolinux/7/7.8/updates/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux7
```

For EuroLinux 7.7:
```ini
[eurolinux7-base]
name=Eurolinux 7.8 Base Vault
baseurl=https://vault.cdn.euro-linux.com/legacy/eurolinux/7/7.7/os/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux7

[eurolinux7-updates]
name=Eurolinux 7.8 Updates Vault
baseurl=https://vault.cdn.euro-linux.com/legacy/eurolinux/7/7.7/updates/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux7
```


## Removing subscription packages

Since the EuroLinux 7 repositories were behind the paywall, you used to need
the subscription that used `rhn_register` or `el_register` commands and ancient
EuroMan service or local mirror of the system. EuroMan was sunsetted with
EuroLinux 7 EOL.


You can confidently remove the subscription-related packages (as long as you
are not using custom Spacewalk/EuroMan Forman/EuroMan) with the following
command:

```bash
sudo yum remove -y rhn* subscription*
```
