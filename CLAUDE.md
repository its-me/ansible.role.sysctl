# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is an Ansible role (`sysctl`) for setting kernel sysctl variables on target hosts using the `ansible.posix.sysctl` module.

## Linting

CI runs ansible-lint via a GitLab CI component:

```yaml
include:
  - component: $CI_SERVER_FQDN/components/ansible/lint@~latest
```

Run locally with:

```bash
ansible-lint
```

## Role Variables

| Variable | Default | Description |
|---|---|---|
| `sysctl` | `{}` | Dict of sysctl key/value pairs to set |
| `sysctl_file` | `sysctl` | Config filename (without `.conf`). If set to `sysctl` (the role name), writes to `/etc/sysctl.conf`; otherwise writes to `/etc/sysctl.d/<name>.conf` |
| `sysctl_reload` | `false` | Whether to reload sysctl after setting values |

The config path logic is computed in `defaults/main.yml` via `sysctl_config_dir` and `sysctl_config`: if `sysctl_file == role_name` ("sysctl"), the file is `/etc/sysctl.conf`; otherwise it is `/etc/sysctl.d/{{ sysctl_file }}.conf`.

The task uses `become: true` — the play or playbook calling this role must allow privilege escalation.

## Usage Example

```yaml
# inventory or host_vars
sysctl:
  net.ipv4.conf.default.rp_filter: 1
```
