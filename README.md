# ansible-role-packages

An Ansible role for installing packages on Linux systems. Supports `apt`,
`dnf`, `zypper`, and `pacman` package managers with a consistent, idempotent
interface across distributions.

## Requirements

- Ansible >= 2.14
- `community.general` collection (for zypper and pacman targets)

## Role Variables

All variables default to empty lists and are safe to omit.

### Package manager base packages

Distro-specific baseline packages. Only the list matching the current host's
package manager is used.

| Variable | Package manager |
|---|---|
| `apt_base_packages` | Debian / Ubuntu |
| `dnf_base_packages` | Fedora / RHEL / CentOS |
| `zypper_base_packages` | openSUSE / SLES |
| `pacman_base_packages` | Arch Linux |

### Shared package lists

Applied to every host regardless of package manager.

| Variable | Scope | Typical source |
|---|---|---|
| `package_list` | All hosts | `group_vars/all` |
| `package_list_group` | Host group | `group_vars/<group>` |
| `package_list_host` | Single host | `host_vars/<host>` |

All four sources are merged and deduplicated before installation. The
combination order (lowest to highest specificity) is:

```
<mgr>_base_packages + package_list + package_list_group + package_list_host
```

## Supported Platforms

| Distribution | Package manager |
|---|---|
| Debian / Ubuntu | apt |
| Fedora | dnf |
| RHEL / CentOS 8+ | dnf |
| openSUSE / SLES | zypper |
| Arch Linux | pacman |

## Example Playbook

```yaml
- hosts: all
  roles:
    - role: glennbrown.packages
      vars:
        package_list:
          - git
          - curl
          - vim

        apt_base_packages:
          - apt-transport-https
          - ca-certificates

        dnf_base_packages:
          - epel-release

        package_list_group:
          - htop

        package_list_host:
          - strace
```

### Using group_vars / host_vars

```yaml
# group_vars/webservers.yml
package_list_group:
  - nginx
  - certbot

# host_vars/db01.yml
package_list_host:
  - postgresql-client
```

## Dependencies

No role dependencies. The `community.general` collection must be installed
when targeting zypper or pacman hosts:

```bash
ansible-galaxy collection install community.general
```

## License

MIT

## Author

Glenn Brown

---

*Inspired by [GROG/ansible-role-package](https://github.com/GROG/ansible-role-package).*
