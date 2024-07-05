# Accessing EuroLinux 6.10 Vaulted Repositories

## EuroLinux 6 EOL   2024-06-30

EuroLinux 6 reached its end of life on November 30, 2020; EuroLinux 6 ELS
reached EOL on 2024-06-30 and is no longer supported. This means critical
security updated are no longer provided, making your system highly vulnerable
to security threats.


Upgrading might not always be possible due to reasons like replicating a
specific production environment in development, supporting legacy systems, or
strict software compatibility requirements. In such cases, while accessing the
vaulted repositories is an option, be aware of the inherent security risks.


## EuroLinux 6 vault

Using the following gist is a straightforward way to access the EuroLinux 6
vault:

<script src="https://gist.github.com/AlexBaranowski/cdff6f0c45a50f16ff485813518e44b6.js"></script>

Or manually add the following to `/etc/yum.repos.d/eurolinux-6-vault-repos.repo`:

```ini
[eurolinux6-base]
name=Eurolinux 6 Base Vault
baseurl=https://vault.cdn.euro-linux.com/legacy/eurolinux/6/6.10/BaseOS/x86_64/os/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux

[eurolinux6-extras]
name=Eurolinux 6 Updates Vault
baseurl=https://vault.cdn.euro-linux.com/legacy/eurolinux/6/6.10/Extras/x86_64/os/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux
```

If the GPG key is not present on your system, you can add it with the following
command:

```bash
curl -o /etc/pki/rpm-gpg/RPM-GPG-KEY-eurolinux https://fbi.cdn.euro-linux.com/security/RPM-GPG-KEY-eurolinux
```

## Removing subscription packages

Previously, due to EuroLinux 6 repositories being paywalled, a subscription was
required. This involved tools like `rhn_register` or `el_register` alongside
the EuroMan service or a local mirror. With EuroMan's sunsetting alongside
EuroLinux 6 ELS EOL, these subscription-related packages are no longer
necessary (unless using custom Spacewalk/EuroMan Forman/EuroMan). You can
safely remove them using the following command:

```bash
sudo yum remove -y rhn* subscription*
```
