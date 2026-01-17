[![Actions Status - Master](https://github.com/juju4/ansible-harden-systemd/workflows/AnsibleCI/badge.svg)](https://github.com/juju4/ansible-harden-systemd/actions?query=branch%3Amaster)
[![Actions Status - Devel](https://github.com/juju4/ansible-harden-systemd/workflows/AnsibleCI/badge.svg?branch=devel)](https://github.com/juju4/ansible-harden-systemd/actions?query=branch%3Adevel)

# harden-systemd ansible role

Enforce stricter settings for services/daemons using various systemd security options.
This mostly focused on network services.

Warning: Test to your context and adapt else it can break things, especially more for cloud agents which can have very large functions.
At minimum, service should start.

## Requirements & Dependencies

### Ansible
It was tested on the following versions:
 * 2.11

### Operating systems

Tested on Ubuntu 18.04, 20.04 and 21.04, server usage and pinephone (mobian bookworm).
This should work on all systemd distributions but may get warnings for unsupported keyword for older ones.

## Example Playbook

Just include this role in your list.
For example

```
- host: myhost
  roles:
    - juju4.harden-systemd
```

## Variables

```
harden_systemd_list:
  # defaults Ubuntu 20.04
  - accounts-daemon
  - dbus
  - rsync
  - rsyslog
  # defaults Ubuntu 21.04
  - packagekit
  #
  # pinephone
  # - phosh
  # non-default
  - avahi-daemon
  - bluetooth
  - NetworkManager
  - wpa_supplicant
  # cloud agents
  # - do-agent
  # - omsagent
```

## Continuous integration

```
$ pip install molecule docker
$ molecule test
$ MOLECULE_DISTRO=ubuntu:20.04 molecule test --destroy=never
```

## Troubleshooting & Known issues

* intermittent 'rsyslog.service: Preparation of eBPF allow maps failed: Operation not permitted'
May try with
```
AmbientCapabilities=CAP_BPF CAP_PERFMON
```
and restarting service with a `systemctl daemon-reload` before.
Still not clear why some service would need eBPF.

* To apply changes, either enable task notify in systemd.yml, either do manual way (service by service or system restart). It seems there is a bug in ansible notify mechanism when used with loop making few items change triggering a notification for all items.

## References

* https://www.freedesktop.org/software/systemd/man/systemd.exec.html
* https://wiki.debian.org/ServiceSandboxing
* https://github.com/krathalan/systemd-sandboxing/
* See also: [juju4.harden_nginx](https://github.com/juju4/ansible-harden-nginx/blob/master/templates/systemd-override.conf.j2), [juju4.harden_apache](https://github.com/juju4/ansible-harden-apache/blob/master/templates/systemd-override.conf.j2), [juju4.squid](https://github.com/juju4/ansible-squid/blob/master/templates/systemd-override.conf.j2), [juju4.tinyproxy](https://github.com/juju4/ansible-tinyproxy/blob/master/templates/systemd-override.conf.j2) and more
* [Hardening OpenLDAP on Linux with AppArmor and systemd, 2018](https://openldap.org/conf/odd-tuebingen-2018/Michael1.pdf), [ae-slapd.service.j2](https://code.stroeder.com/AE-DIR/ansible-ae-dir-server/src/branch/main/templates/systemd/ae-slapd.service.j2), [defaults aedir_systemd_hardening](https://code.stroeder.com/AE-DIR/ansible-ae-dir-server/src/branch/main/defaults/main.yml#L161)
* [Linux speedrun: systemd, 2025](https://michaelboelen.com/presentations/): SystemCallLog

## License

BSD 2-clause
