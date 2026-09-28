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

## Dependencies

- None
