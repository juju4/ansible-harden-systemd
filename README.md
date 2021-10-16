[![Actions Status - Master](https://github.com/juju4/ansible-harden-systemd/workflows/AnsibleCI/badge.svg)](https://github.com/juju4/ansible-harden-systemd/actions?query=branch%3Amaster)
[![Actions Status - Devel](https://github.com/juju4/ansible-harden-systemd/workflows/AnsibleCI/badge.svg?branch=devel)](https://github.com/juju4/ansible-harden-systemd/actions?query=branch%3Adevel)

# harden-systemd ansible role

Enforce stricter settings for services/daemons using various systemd security options.
This mostly focused on network services.

Warning: Test to your context and adapt else it can break things.

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

N/A

## License

BSD 2-clause
