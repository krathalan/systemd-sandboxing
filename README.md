# systemd-sandboxing
Drop-in `.conf` files for sandboxing systemd services.

## Explanation of options
View the `guide.conf` file in the root of this repository for a short explanation of each option. Most comments come from `systemd.exec(5)` and `systemd.resource-control(5)` man pages. Most options come from all of the options that `systemd-analyze security` uses to score a service.

## Installation
Simply copy the `*.service.d` files to `/etc/systemd/system/`.

### Arch Linux
If you want to install as a package, I maintain a PKGBUILD:

```
$ git clone https://git.sr.ht/~krathalan/pkgbuilds
$ cd pkgbuilds/krathalans-systemd-sandboxing-git
$ less PKGBUILD # Always inspect PKGBUILDS before running makepkg!
$ makepkg -i
```

This package installs `.conf` files to `/usr/lib/systemd/system/*.service.d/`.

## Services

- bluetooth
- dovecot
- fail2ban
- iwd
- nginx
- opendkim
- opendmarc
- pkgstats
- postfix
- sshd

Some sandboxing options, like those used for nginx, opendkim, and opendmarc, assume you are running the service as an unprivileged user. There is also a `user.conf` file in their directory, in addition to the regular `hardening.conf`.

See these links for more information:

https://wiki.archlinux.org/index.php/Nginx#Running_unprivileged_using_systemd

https://wiki.archlinux.org/index.php/OpenDKIM#Security