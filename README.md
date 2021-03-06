# Ansible role to manage OpenWRT devices

WARNING: Work In Progress, use at your own risk.

This role helps with managing OpenWRT devices. It is useful if your device
supports ext-root, so more packages can be installed.

## Requirements

None. The required packages are managed by the role.

## Role Variables

- From `defaults/main.yml`

```yaml
---
openwrt_ntp_servers:
  - 0.europe.pool.ntp.org
  - 1.europe.pool.ntp.org
  - 2.europe.pool.ntp.org
  - 3.europe.pool.ntp.org
openwrt_ula_prefix: 'aaaa:1111:bbb::/48'
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
- name: OpenWRT | Wireless freedom | Bootstrap
  hosts: openwrt
  remote_user: root
  gather_facts: false
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

  tasks:

    - name: BOOTSTRAP | Validate bootstrap by querying data
      setup:

    - name: BOOTSTRAP | Ensure access via non-root user
      include_role:
        name: ansible-os-openwrt
        tasks_from: sysadm

- name: OpenWRT | Wireless freedom
  hosts: openwrt
  remote_user: sysadm
  become: true
  gather_facts: true
  vars:
    openwrt_ntp_servers:
      - 0.europe.pool.ntp.org
      - 1.europe.pool.ntp.org
      - 2.europe.pool.ntp.org
      - 3.europe.pool.ntp.org
      - 0.es.pool.ntp.org
      - 1.es.pool.ntp.org
      - 2.es.pool.ntp.org
      - 3.es.pool.ntp.org
    openwrt_ula_prefix: 'fdee:2008:fd33::/48'

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

[David Sastre Medina](mailto:d.sastre.medina@gmail.com?subject=[GitHub]%20Ansible%20OpenWRT%20role)
