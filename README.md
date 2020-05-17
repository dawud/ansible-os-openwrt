# Ansible role to manage OpenWRT devices

WARNING: Work In Progress, use at your own risk.

This role helps with managing OpenWRT devices. It is useful if your device
supports ext-root, so more packages can be installed.

## Requirements

None. The required packages are managed by the role.

## Role Variables

- From `defaults/main.yml`

```yaml
TBC
```

- From `vars/main.yml`

```yaml
TBC
```

## Dependencies

None.

## Example Playbook

Example of how to use this role:

```yaml
---
- name: OpenWRT | Wireless freedom
  hosts: openwrt
  remote_user: root
  gather_facts: false
  vars:
    openwrt_packages:
      - blkid
      - block-mount
      - diffutils
      - e2fsprogs
      - ethtool
      - fdisk
      - htop
      - iftop
      - ip-full
      - ipset
      - kmod-fs-ext4
      - kmod-macsec
      - kmod-usb-ohci
      - kmod-usb-storage
      - kmod-usb-uhci
      - less-wide
      - luci-mod-rpc
      - python3
      - zoneinfo-europe
    openwrt_ntp_servers:
      - 0.europe.pool.ntp.org
      - 1.europe.pool.ntp.org
      - 2.europe.pool.ntp.org
      - 3.europe.pool.ntp.org
    openwrt_ula_prefix: 'aaaa:1111:bbb::/48'

  pre_tasks:

    - name: BOOTSTRAP | Probe python installation
      raw: command -v python3 || true
      register: python_available
      changed_when: false

    - name: BOOTSTRAP | Probe python installation version
      raw: python3 --version
      register: python_version
      when: "'python' in python_available.stdout"
      changed_when: false

    - name: BOOTSTRAP | Bootstrap an OpenWRT host without python installed
      raw: opkg install python3
      when: "'python' not in python_available.stdout"

    - setup:

  roles:
    - ansible-os-openwrt
```

## Contributing

This repository uses [git-flow](http://nvie.com/posts/a-successful-git-branching-model/).
To contribute to the role, create a new feature branch (`feature/foo_bar_baz`),
and submit a pull request targeting the `develop` branch.

Happy hacking!

## License

MIT

## Author Information

[David Sastre Medina](d.sastre.medina@gmail.com)
