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
- dictd
- dovecot
- fail2ban
- iwd
- nginx
- opendkim
- opendmarc
- org.cups.cupsd
- pkgstats
- postfix
- usbguard

### cupsd
The cupsd overrides have only been tested with an HP printer on the local network. Printers from other manufacturers or printers connected via a different interface (e.g. USB, SAMBA, etc.) may not work. Patches accepted.

### dictd
The dictd overrides assume that you only want to access the dictd server from your local machine. The pid file is somewhat useless, so its default location is not writable with these hardening options. Here is an example `dictd.conf` snippet for these hardening options:

```
global {
    site site.info
    pid_file /dev/null
}

# who's allowed.  You might want to change this.
access {
  allow 127.0.0.*
}
```

This snippet makes dictd not create a pid file and only allow access from the localhost.

### services run as unprivileged user
Some sandboxing options, like those used for nginx, opendkim, and opendmarc, assume you are running the service as an unprivileged user. There is also a `user.conf` file in their directory, in addition to the regular `hardening.conf`.

See these links for more information:

https://wiki.archlinux.org/index.php/Nginx#Running_unprivileged_using_systemd

https://wiki.archlinux.org/index.php/OpenDKIM#Security

### other caveats

The `sshd` override only adds IPAccounting, no sandboxing.

The `systemd-logind` override only adds the service to the `proc` group, un-breaking the service when `/proc` is mounted with `hidepid=2,gid=proc`.

The `systemd-networkd` override only adds `apparmor.service` to the `After=` option, so that the service doesn't start before its AppArmor profile is loaded.y
