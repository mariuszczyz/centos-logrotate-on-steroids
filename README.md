# Logrotate on steroids

This role configures logrotate for high-compression log rotation. It rotates logs daily, compresses them with xz at maximum compression, and keeps 30 days of archives. It also manages the rsyslog logrotate snippet.

## Requirements

- ansible-core 2.15 or newer.
- Target hosts must use dnf. This covers Enterprise Linux 9 and 10, and Fedora.

## Supported platforms

| Distribution    | Versions |
|-----------------|----------|
| Rocky Linux     | 9, 10    |
| AlmaLinux       | 9, 10    |
| CentOS Stream   | 9, 10    |
| Fedora          | 44, 45   |

## Role Variables

| Variable                    | Type | Default         | Description                                        |
|-----------------------------|------|-----------------|----------------------------------------------------|
| `logrotate_frequency`       | str  | `daily`         | Rotation interval. Choices: `daily`, `weekly`, `monthly`. |
| `logrotate_rotate`          | int  | `30`            | Number of rotated archives to keep.                |
| `logrotate_compress`        | bool | `true`          | Compress rotated logs.                             |
| `logrotate_compresscmd`     | str  | `/usr/bin/xz`   | Compression binary.                                |
| `logrotate_compressoptions` | str  | `-z -9`         | Options passed to the compression binary.          |
| `logrotate_compressext`     | str  | `.xz`           | Extension of compressed archives.                  |
| `logrotate_uncompresscmd`   | str  | `/usr/bin/unxz` | Decompression binary.                              |
| `logrotate_manage_syslog`   | bool | `true`          | Manage the rsyslog logrotate snippet.              |

Ansible validates these parameters against `meta/argument_specs.yml` when a playbook uses `include_role` or `import_role`. An invalid value stops the run with an error that names the parameter.

## Dependencies

None.

## Example Playbook

Install the role from Ansible Galaxy:

```bash
ansible-galaxy install mariuszczyz.centos-logrotate-on-steroids
```

In playbook.yml:

```yaml
- hosts: servers
  tasks:
    - name: Apply log rotation settings
      ansible.builtin.include_role:
        name: mariuszczyz.centos-logrotate-on-steroids
      vars:
        logrotate_frequency: weekly
        logrotate_rotate: 60
```

Run the playbook:

```bash
ansible-playbook -i hosts playbook.yml --user root --ask-pass --limit=hostname
```

Use `--become` instead when your sudo policy allows it:

```bash
ansible-playbook -i hosts playbook.yml --user username --become --ask-become-pass --limit=hostname
```

The role overwrites `/etc/logrotate.conf` and `/etc/logrotate.d/rsyslog` on every run. Change behavior through the role variables. Do not edit those files by hand.

## Testing

The repository includes a Molecule test suite that runs the role in Podman containers on every supported platform.

```bash
python3 -m venv .venv && . .venv/bin/activate
pip install -r requirements.txt
molecule test
```

You need a local Podman installation. The suite creates and destroys one container per platform.

## License

[MIT](LICENSE)

## Author Information

Mariusz Czyz
