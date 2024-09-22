# Ansible Role: Nomad Installation

This Ansible role installs and configures [Nomad](https://www.nomadproject.io/) by HashiCorp on a Linux system. It supports downloading and verifying the specified version of Nomad for the appropriate architecture, and it installs the Nomad binary.

## Features

- Installs the specified version of Nomad (configurable via `nomad_installation_version`).
- Supports multiple architectures (`amd64`, `arm64`) and auto-detects the system architecture.
- Downloads and verifies the Nomad binary using SHA256 checksums.
- Configurable binary installation path (default is `/usr/local/bin`).
- Ensures proper file ownership and permissions for the installed Nomad binary.
- Designed to work on various Linux distributions.

## Requirements

- Ansible 2.17.4 or higher.
- A Linux distribution.

## Role Variables

Here are the role variables and their default values. You will need to override them in your playbook or inventory to suit your environment:

| Variable | Description | Default |
| - | - | - |
| nomad_installation_user_temp_dir | Custom temporary directory for Nomad artifacts. If set, this directory will be used; otherwise, a temporary directory will be created automatically. | - |
| nomad_installation_version | Version of Nomad to be installed. | `1.8.4` |
| nomad_installation_bin_dir | Directory where the Nomad binary will be installed. | `/usr/local/bin` |
| nomad_installation_user | User who will own the Nomad binary. | `{{ ansible_user }}` |
| nomad_installation_group | Group that will own the Nomad binary. | `{{ ansible_user }}` |

## Example Playbook

```yaml
- hosts: all
  become: true
  roles:
    - role: dsegurag.nomad_installation_installation
      vars:
        nomad_installation_version: "1.8.4"
```

## Tasks Overview

1. Creates a temporary directory to store the downloaded Nomad artifacts.
2. Downloads the SHA256 checksum file for the specified Nomad version.
3. Verifies the checksum of the downloaded Nomad package.
4. Downloads the Nomad package (if not already downloaded).
5. Unarchives the Nomad binary from the package.
6. Copies the Nomad binary to the specified directory (`/usr/local/bin` by default).
7. Ensures the correct file ownership and permissions for the Nomad binary.

**Note**:

- All download, extraction, and checksum verification tasks are delegated to the localhost, meaning they run on the control machine rather than the target hosts.
- This role **cannot run in check mode**. Since it involves downloading files, verifying checksums, and creating temporary directories, these actions require actual changes to the filesystem and network activity, which check mode doesn't allow.

## Dependencies

- None.

## License

MIT License

## Author Information

This role was created by Daniel Segura.
