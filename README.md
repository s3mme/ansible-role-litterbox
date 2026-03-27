# ansible-role-litterbox

Ansible role that installs [LitterBox](https://github.com/BlackSnufkin/LitterBox) on Windows. LitterBox is a secure sandbox environment for testing payloads against detection mechanisms.

## Requirements

- **Windows** target hosts
- **Ansible** >= 2.14
- Ansible collections: `ansible.windows`, `chocolatey.chocolatey`

## Role Variables

### Defaults (`defaults/main.yml`)

| Variable                       | Default                                         | Description                                  |
| ------------------------------ | ----------------------------------------------- | -------------------------------------------- |
| `litterbox_install_dir`        | `C:\Tools\LitterBox`                            | Installation directory                       |
| `litterbox_repo_url`           | `https://github.com/BlackSnufkin/LitterBox.git` | Git repository URL                           |
| `litterbox_repo_version`       | `main`                                          | Git branch/tag to clone                      |
| `litterbox_python_version`     | `3.11`                                          | Python version to install via Chocolatey     |
| `litterbox_python_install_dir` | `C:\Python311`                                  | Python installation path                     |
| `litterbox_manage_config`      | `false`                                         | Whether to template and deploy `config.yaml` |

### Configuration variables

> [!NOTE]
> only applied when `litterbox_manage_config: true`

| Variable                       | Default                                 | Description                   |
| ------------------------------ | --------------------------------------- | ----------------------------- |
| `litterbox_host`               | `127.0.0.1`                             | Listen address                |
| `litterbox_port`               | `1337`                                  | Listen port                   |
| `litterbox_debug`              | `false`                                 | Enable debug mode             |
| `litterbox_max_file_size`      | `104857600`                             | Max upload file size (bytes)  |
| `litterbox_allowed_extensions` | `[exe, dll, bin, docx, xlsx, lnk, sys]` | Allowed file types for upload |

Scanner variables:

> [!NOTE]
> Default: all set to `true`

| Variable                                   | Description                          |
| ------------------------------------------ | ------------------------------------ |
| `litterbox_scanner_holygrail_enabled`      | Enable HolyGrail scanner             |
| `litterbox_scanner_static_yara_enabled`    | Enable static Yara analysis          |
| `litterbox_scanner_checkplz_enabled`       | Enable CheckPlz scanner              |
| `litterbox_scanner_stringnalyzer_enabled`  | Enable Stringnalyzer scanner         |
| `litterbox_scanner_dynamic_yara_enabled`   | Enable dynamic Yara analysis         |
| `litterbox_scanner_pe_sieve_enabled`       | Enable PE-Sieve scanner              |
| `litterbox_scanner_hollows_hunter_enabled` | Enable HollowsHunter scanner         |
| `litterbox_scanner_moneta_enabled`         | Enable Moneta scanner                |
| `litterbox_scanner_patriot_enabled`        | Enable Patriot scanner               |
| `litterbox_scanner_hsb_enabled`            | Enable Hunt-Sleeping-Beacons scanner |
| `litterbox_scanner_rededr_enabled`         | Enable RedEdr scanner                |

## Example Playbook

```yaml
- hosts: windows_sandbox
  roles:
    - role: ansible-role-litterbox
```

With managed configuration:

```yaml
- hosts: windows_sandbox
  roles:
    - role: ansible-role-litterbox
      litterbox_manage_config: true
      litterbox_host: "0.0.0.0"
      litterbox_port: 8080
      litterbox_scanner_rededr_enabled: false
```

## Testing

This role includes [Molecule](https://molecule.readthedocs.io/) tests using Vagrant with a Windows Server 2022 VM:

```bash
molecule test
```

## License

MIT

## Author Information

- [s3mme.com](https://s3mme.com/)
