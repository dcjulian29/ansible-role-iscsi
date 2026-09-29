# Ansible Role: iscsi

[![Lint](https://github.com/dcjulian29/ansible-role-iscsi/actions/workflows/lint.yml/badge.svg)](https://github.com/dcjulian29/ansible-role-iscsi/actions/workflows/lint.yml) [![GitHub Issues](https://img.shields.io/github/issues-raw/dcjulian29/ansible-role-iscsi.svg)](https://github.com/dcjulian29/ansible-role-iscsi/issues)

Ansible role to attach iSCSI volumes and prepare their filesystems

## Requirements

- Active Internet Connection.

## Installation

To use, use `requirements.yml` with the following git source:

```yaml
---
roles:
- name: dcjulian29.iscsi
  src: https://github.com/dcjulian29/ansible-role-iscsi.git
  version: main
  ```

Then download it with `ansible-galaxy`:

```shell
ansible-galaxy install -r requirements.yml
```

## Role Variables

| Variable | Default | Description |
| --- | --- | --- |
| `iscsi_initiator` | `""` | IQN written to `/etc/iscsi/initiatorname.iscsi`. Pin it so a recreated host logs in without changing the target's initiator group. Empty leaves the generated name alone. |
| `iscsi_port` | `3260` | Portal port used when a volume does not set its own. |
| `iscsi_volumes` | `[]` | Volumes to attach, one entry per target. |

Each entry in `iscsi_volumes` accepts:

| Key | Required | Default | Description |
| --- | --- | --- | --- |
| `portal` | yes | | Address of the iSCSI portal. |
| `target` | yes | | Target IQN. |
| `port` | no | `iscsi_port` | Portal port. |
| `lun` | no | `0` | LUN of the volume on the target. |
| `label` | no | | Filesystem label. When set, a filesystem is created on a blank volume; an existing filesystem is left alone. When omitted, the volume is only attached. |
| `fstype` | no | `ext4` | Filesystem type to create when `label` is set. |

The role attaches and formats volumes but does not mount them. Mount by label
with `dcjulian29.fstab`, listed after this role, and include `_netdev` in the
options so the mount waits for the network:

```yaml
iscsi_initiator: iqn.2026-09.com.example:app
iscsi_volumes:
  - portal: 192.168.1.100
    target: iqn.2005-10.org.freenas.ctl:app
    label: app

fstab_mounts:
  - fstype: ext4
    opts: defaults,noatime,_netdev
    path: /srv/app
    src: LABEL=app
    state: mounted
```

## Dependencies

- `community.general` collection
